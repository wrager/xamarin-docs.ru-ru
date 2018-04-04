---
title: Иерархическая навигация
description: Класс NavigationPage предоставляет качества иерархической навигации, где пользователь является возможность перемещаться по страницам, вперед и назад в случае необходимости. Класс реализует интерфейс навигации как стек последним поступил — первым обслужен (LIFO) объектов страницы. В этой статье показано, как использовать класс NavigationPage выполнить переход в стеке страниц.
ms.prod: xamarin
ms.assetid: C8A5EEFF-5A3B-4163-838A-147EE3939FAA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2017
ms.openlocfilehash: afaf0c702cdba1ba9c5d2c9d158501c50501f910
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="hierarchical-navigation"></a>Иерархическая навигация

_Класс NavigationPage предоставляет качества иерархической навигации, где пользователь является возможность перемещаться по страницам, вперед и назад в случае необходимости. Класс реализует интерфейс навигации как стек последним поступил — первым обслужен (LIFO) объектов страницы. В этой статье показано, как использовать класс NavigationPage выполнить переход в стеке страниц._

В этой статье рассматриваются следующие вопросы:

- [Выполнение навигации](#Performing_Navigation) — Создание корневую страницу, помещения страницы по стеку навигации, выталкивания страниц из стека переходов и анимации переходов.
- [Передача данных при переходе](#Passing_Data_when_Navigating) — передача данных через конструктор страницы и `BindingContext`.
- [Управление стек навигации](#Manipulating_the_Navigation_Stack) — управление стека путем вставки или удаления страниц.

## <a name="overview"></a>Обзор

Для перемещения от одной страницы в другую, приложение будет передавать новую страницу в стек навигации, где он становится активной страницы, как показано на следующей схеме:

![](hierarchical-images/pushing.png "Помещает в стек переходов страницы")

Чтобы вернуться на предыдущую страницу, приложение отобразит текущей страницы из стека навигации и новая страница верхних станет активной, как показано на следующей схеме:

![](hierarchical-images/popping.png "Выталкивания страницы из стека навигации")

Представляемые методы навигации [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) свойство для какого-либо [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) производных типов. Эти методы предоставляют возможность для принудительной отправки страницы в стек переходов pop страницы из стека навигации, а также для выполнения со стеком.

<a name="Performing_Navigation" />

## <a name="performing-navigation"></a>Выполнение навигации

При иерархической навигации класс [`NavigationPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) используется для перехода по стеку объектов [`ContentPage`](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Следующих снимках экрана Показать основные компоненты `NavigationPage` на каждой платформе:

![](hierarchical-images/navigationpage-components.png "Компоненты NavigationPage")

Макет [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) зависит от платформы:

- На iOS, присутствует панель навигации в верхней части страницы, отображается заголовок и имеющего *обратно* кнопки, которая возвращает к предыдущей странице.
- В Android присутствует панель навигации в верхней части страницы, отображается заголовок, значок и *обратно* кнопки, которая возвращает к предыдущей странице. Значок определяется в `[Activity]` атрибут, который оформляет `MainActivity` класса в проекте специфический для платформы Android.
- На Windows Phone панели навигации в верхней части страницы, на которой отображается заголовок отсутствует. Windows Phone не имеет *обратно* кнопку на панели навигации, поскольку на экране *обратно* кнопка присутствует в нижней части экрана.

На всех платформах, значение [ `Page.Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Title/) свойство отображается как заголовок страницы.

> [!NOTE]
> Рекомендуется `NavigationPage` должен быть заполнен `ContentPage` только экземпляр.

### <a name="creating-the-root-page"></a>Создание корневой страницы

Первая страница, добавленная в стек навигации, называется *корневой* страницей приложения, что демонстрируется в следующем примере кода:

```csharp
public App ()
{
  MainPage = new NavigationPage (new Page1Xaml ());
}
```

В результате `Page1Xaml` [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) экземпляра, помещаемых в стек навигации, где оно станет активной страницы и корневую страницу приложения. Это показано на следующих снимках экрана:

![](hierarchical-images/mainpage.png "Страница корневого навигации стека")

> [!NOTE]
> [ `RootPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.NavigationPage.RootPage/) Свойство [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) экземпляр предоставляет доступ к первой странице в стеке навигации.

### <a name="pushing-pages-to-the-navigation-stack"></a>Помещает в стек навигации страницы

Для перехода к `Page2Xaml`, необходимо вызвать [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) метод [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) свойства текущей страницы, как демонстрируется в следующем примере кода:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PushAsync (new Page2Xaml ());
}
```

В результате в стек навигации помещается экземпляр `Page2Xaml`, где он становится активной страницей. Это показано на следующих снимках экрана:

![](hierarchical-images/secondpage.png "Страница стек навигации")

Когда [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) вызове метода, происходят следующие события:

- Вызов страницы `PushAsync` имеет его [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) переопределение вызывается.
- На странице, куда выполняется переход его [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) переопределение вызывается.
- `PushAsync` Завершения задачи.

Тем не менее точный порядок, в котором эти события происходят — зависит от используемой платформы. Дополнительные сведения см. в разделе [Глава 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Чарльз Петцольд Xamarin.Forms книги.

> [!NOTE]
> Вызовы [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) и [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) переопределения не может рассматриваться как гарантированную указаний по страницам. Например, для операций ввода-вывода `OnDisappearing` переопределение вызывается на активной странице, когда приложение завершает работу.

### <a name="popping-pages-from-the-navigation-stack"></a>Извлечение страниц из стека навигации

Активная страница может быть извлечена из стека навигации путем нажатия кнопки *Назад* на устройстве, причем это может быть как физическая кнопка, так и кнопка на экране.

Чтобы вернуться на исходную страницу программным образом, экземпляр `Page2Xaml` должен вызвать метод [`PopAsync`](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/), как показано в следующем примере кода:

```csharp
async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopAsync ();
}
```

В результате экземпляр `Page2Xaml` удаляется из стека навигации, а активной становится верхняя страница в нем. Когда [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) вызове метода, происходят следующие события:

- Вызов страницы `PopAsync` имеет его [ `OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing/) переопределение вызывается.
- На странице, возвращается его [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing/) переопределение вызывается.
- `PopAsync` Возвращает задач.

Тем не менее точный порядок, в котором эти события происходят — зависит от используемой платформы. Дополнительные сведения см. [Глава 24](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf) Чарльз Петцольд Xamarin.Forms книги.

А также [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PushAsync(Xamarin.Forms.Page)/) и [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) методы, [ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) также предоставляет свойства каждой страницы [ `PopToRootAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopToRootAsync()/) метод, как показано в следующем примере кода:

```csharp
async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  await Navigation.PopToRootAsync ();
}
```

Этот метод извлекает все, кроме корневой [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) из стека переходов, таким образом делая корневую страницу активная страница приложения.

### <a name="animating-page-transitions"></a>Страница проходит анимации

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Также предоставляет свойства каждой страницы переопределенный принудительной отправки и pop методы, которые включают `boolean` параметр, который управляет отображением страницы анимация во время перехода, как показано в следующем коде Пример:

```csharp
async void OnNextPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PushAsync (new Page2Xaml (), false);
}

async void OnPreviousPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopAsync (false);
}

async void OnRootPageButtonClicked (object sender, EventArgs e)
{
  // Page appearance not animated
  await Navigation.PopToRootAsync (false);
}
```

Установка `boolean` параметр `false` отключает переход страниц анимацию, установив параметр в `true` позволяет анимации переход страниц, при условии, что он поддерживается используемой платформой. Тем не менее помещать и извлекать методы, которые не хватает этот параметр включает анимации по умолчанию.

<a name="Passing_Data_when_Navigating" />

## <a name="passing-data-when-navigating"></a>Передача данных при переходе

Иногда она необходима для страницы для передачи данных на другую страницу во время навигации. Два способа для решения этой проблемы происходит обмен данными через конструктор страницы и задав новую страницу [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) к данным. Каждый теперь обсуждаются в свою очередь.

### <a name="passing-data-through-a-page-constructor"></a>Передача данных с помощью конструктора страницы

Проще всего для передачи данных на другую страницу во время навигации осуществляется через страницу параметра конструктора, как показано в следующем примере кода:

```csharp
public App ()
{
  MainPage = new NavigationPage (new MainPage (DateTime.Now.ToString ("u")));
}
```

Этот код создает `MainPage` экземпляра, передача в текущую дату и время в формате ISO8601, который упаковывается в [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) экземпляра.

`MainPage` Экземпляр получает данные с помощью параметра конструктора, как показано в следующем примере кода:

```csharp
public MainPage (string date)
{
  InitializeComponent ();
  dateLabel.Text = date;
}
```

Данные отображаются на странице, задав [ `Label.Text` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.Text/) свойства, как показано на следующем снимке экрана:

![](hierarchical-images/passing-data-constructor.png "Данные, передаваемые через конструктор страницы")

### <a name="passing-data-through-a-bindingcontext"></a>Передача данных через объект BindingContext

Альтернативный подход для передачи данных на другую страницу во время перехода является установка на новую страницу [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) к данным, как показано в следующем примере кода:

```csharp
async void OnNavigateButtonClicked (object sender, EventArgs e)
{
  var contact = new Contact {
    Name = "Jane Doe",
    Age = 30,
    Occupation = "Developer",
    Country = "USA"
  };

  var secondPage = new SecondPage ();
  secondPage.BindingContext = contact;
  await Navigation.PushAsync (secondPage);
}
```

Этот код задает [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) из `SecondPage` экземпляр `Contact` экземпляра, а затем переходит к `SecondPage`.

`SecondPage` Затем использует привязку данных для отображения `Contact` экземпляра данных, как показано в следующем примере кода XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="PassingData.SecondPage"
             Title="Second Page">
    <ContentPage.Content>
        <StackLayout HorizontalOptions="Center" VerticalOptions="Center">
            <StackLayout Orientation="Horizontal">
                <Label Text="Name:" HorizontalOptions="FillAndExpand" />
                <Label Text="{Binding Name}" FontSize="Medium" FontAttributes="Bold" />
            </StackLayout>
            ...
            <Button x:Name="navigateButton" Text="Previous Page" Clicked="OnNavigateButtonClicked" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

В следующем примере кода показано, как в C#, в котором можно выполнить привязку данных.

```csharp
public class SecondPageCS : ContentPage
{
  public SecondPageCS ()
  {
    var nameLabel = new Label {
      FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
      FontAttributes = FontAttributes.Bold
    };
    nameLabel.SetBinding (Label.TextProperty, "Name");
    ...
    var navigateButton = new Button { Text = "Previous Page" };
    navigateButton.Clicked += OnNavigateButtonClicked;

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
        navigateButton
      }
    };
  }

  async void OnNavigateButtonClicked (object sender, EventArgs e)
  {
    await Navigation.PopAsync ();
  }
}
```

Данные отображаются на странице ряд [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) элементов управления, как показано на следующем снимке экрана:

![](hierarchical-images/passing-data-bindingcontext.png "Данные, передаваемые через BindingContext")

Дополнительные сведения о привязке данных см. в статье [Основы привязки данных](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md).

<a name="Manipulating_the_Navigation_Stack" />

## <a name="manipulating-the-navigation-stack"></a>Управление стеке навигации

[ `Navigation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Navigation/) Предоставляет свойство [ `NavigationStack` ](https://developer.xamarin.com/api/property/Xamarin.Forms.INavigation.NavigationStack/) свойства, из которого можно получить страниц в стеке навигации. Хотя Xamarin.Forms поддерживает доступ к стеку навигации `Navigation` предоставляет свойство [ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) и [ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) методы для управления стека путем вставки страницы или удалив их.

[ `InsertPageBefore` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.InsertPageBefore(Xamarin.Forms.Page,Xamarin.Forms.Page)/) Метод вставляет указанную страницу в стеке навигации перед указанной существующей страницы, как показано на следующей схеме:

![](hierarchical-images/insert-page-before.png "Вставка страницы в стеке навигации")

[ `RemovePage` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.RemovePage(Xamarin.Forms.Page)/) Метод удаляет указанную страницу из стека переходов, как показано на следующей схеме:

![](hierarchical-images/remove-page.png "Удаление страницы из стека навигации")

Эти методы позволяют пользовательский навигационный взаимодействие, такие как замена страницу входа в систему с новой страницы, после успешного входа. В следующем примере кода демонстрируется этот сценарий:

```csharp
async void OnLoginButtonClicked (object sender, EventArgs e)
{
  ...
  var isValid = AreCredentialsCorrect (user);
  if (isValid) {
    App.IsUserLoggedIn = true;
    Navigation.InsertPageBefore (new MainPage (), this);
    await Navigation.PopAsync ();
  } else {
    // Login failed
  }
}

```

Условии, что учетные данные пользователя, правильно ли указаны `MainPage` экземпляр вставляется в стеке навигации перед текущей страницы. [ `PopAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.NavigationPage.PopAsync()/) Метод затем удаляет текущую страницу в стеке навигации с `MainPage` экземпляр становится активной странице.

## <a name="summary"></a>Сводка

В этой статье показано, как использовать [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) класс выполнить переход в стеке страниц. Этот класс предоставляет качества иерархической навигации, где пользователь является возможность перемещаться по страницам, вперед и назад в случае необходимости. Этот класс реализует навигацию на основе стека объектов [`Page`](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) по методу LIFO (последним поступил — первым обслужен).


## <a name="related-links"></a>Связанные ссылки

- [Навигация по страницам](https://developer.xamarin.com/r/xamarin-forms/book/chapter24.pdf)
- [Иерархическое (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/Hierarchical/)
- [PassingData (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/PassingData/)
- [LoginFlow (пример)](https://developer.xamarin.com/samples/xamarin-forms/Navigation/LoginFlow/)
- [Создание знак в потоке экрана в образце Xamarin.Forms (университет Xamarin видео)](http://xamarinuniversity.blob.core.windows.net/lightninglectures/CreateASignIn.zip)
- [Создание знак в поток экрана в Xamarin.Forms (университет Xamarin видео)](https://university.xamarin.com/lightninglectures/how-to-create-a-sign-in-screen-flow-in-xamarinforms)
- [NavigationPage](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/)
