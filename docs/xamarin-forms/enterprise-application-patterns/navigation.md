---
title: Навигация
ms.prod: xamarin
ms.assetid: 4cad57b5-7fe4-4527-a988-d9b60c9620b4
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: aa2e2858e3bb8e435ec3f38bb3d5b249eaa6cba4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="navigation"></a>Навигация

Xamarin.Forms поддерживает навигацию по страницам, что обычно приводит к из взаимодействие пользователя с помощью пользовательского интерфейса или из самого приложения, в результате изменения внутреннего состояния на основе логики. Однако навигации может быть сложным для реализации в приложениях, использующих шаблон Model-View-ViewModel (MVVM), которые должны быть выполнены следующие задачи:

-   Как определить представления навигации, использование метода, который не вводит тесной связи и зависимости между представлениями.
-   Как для координации процесса с помощью которого создается и инициализируется представления, осуществлять переходы. При использовании MVVM, представление и представление модели необходимо создать экземпляр и связанные друг с другом через контекст привязки для этого представления. Если приложение использует контейнер внедрения зависимостей, при создании экземпляра представления и Просмотр моделей может потребоваться механизм определенную конструкцию.
-   Следует ли выполнять навигации первого представления или представления навигации «сначала модель». С навигацией представление первой страницы, чтобы перейти к ссылается на имя типа представления. Во время перехода указанное представление создается вместе с его соответствующей модели представления и другие зависимые службы. Альтернативным подходом является использование Навигация по представлению «сначала модель», где страницы для перехода к ссылается на имя типа модели представления.
-   Как чтобы четко отделить навигационный поведение приложения через представления и Просмотр моделей. Шаблон MVVM позволяет разделить пользовательского интерфейса приложения и его представления и бизнес-логики. Тем не менее поведение приложения будет часто занимает пользовательского интерфейса и презентациям части приложения. Он часто будет инициировать навигации из представления и представления будут заменены в результате навигации. Однако навигации также часто необходимо инициировать или координируемой из модели представления.
-   Способ передачи параметров во время перехода в целях инициализации. Например если пользователь перемещается в представление, чтобы обновить сведения о заказе, данные заказа будет передаются в представление, чтобы он мог отображать правильные данные.
-   Как автоматизированы навигации, чтобы убедиться что всей определенные бизнес-правила. Например пользователи могут получать запрос перед покидая представления, чтобы их можно исправить недопустимые данные или будет предложено отправить или отменить все изменения данных, которые были внесены в пределах представления.

В этой главе решают эти задачи, предоставляя `NavigationService` класс, используемый для выполнения первой модели представления по страницам.

> [!NOTE]
> `NavigationService` Используется приложение, предназначенное только для выполнения иерархическая навигация между экземплярами ContentPage. С помощью службы для перехода между другими типами страницы может привести к непредвиденному поведению.

## <a name="navigating-between-pages"></a>Переход между страницами

Логика перемещения находятся в представлении кода, или в данных привязки модели представления. Размещение логику навигации в представлении может быть простым подходом, не легко тестируемых через модульных тестов. Размещения логики навигации в представлении классов модели означает, что через модульных тестов можно использовать логику. Кроме того модель представления можно затем реализовать логику для управления навигации, чтобы убедиться, что применяются определенные бизнес-правила. Например, приложение может запретить пользователю уходите со страницы без предварительного гарантирует правильность введенных данных.

Объект `NavigationService` класс обычно вызывается из Просмотр моделей, для повышения эффективности тестирования. Тем не менее переход к представлениям из Просмотр моделей требуется Просмотр моделей, ссылающиеся на представления, и особенно представления, которые активное представление модели не связана с, а это не рекомендуется. Таким образом `NavigationService` представленных здесь задает в качестве целевой для перехода к тип модели представления.

Использование мобильного приложения eShopOnContainers `NavigationService` класса для предоставления навигации Просмотр моделей в первую очередь. Этот класс реализует `INavigationService` интерфейс, как показано в следующем примере кода:

```csharp
public interface INavigationService  
{  
    ViewModelBase PreviousPageViewModel { get; }  
    Task InitializeAsync();  
    Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase;  
    Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase;  
    Task RemoveLastFromBackStackAsync();  
    Task RemoveBackStackAsync();  
}
```

Этот интерфейс указывает, что класс, реализующий должен предоставляют следующие методы:

|Метод|Цель|
|--- |--- |
|`InitializeAsync`|Выполняет переход к одной из двух страниц, при запуске приложения.|
|`NavigateToAsync`|Выполняет иерархическая навигация на указанной странице.|
|`NavigateToAsync(parameter)`|Выполняет иерархическая навигация на указанной странице, передавая параметр.|
|`RemoveLastFromBackStackAsync`|Удаляет предыдущую страницу в стеке навигации.|
|`RemoveBackStackAsync`|Удаляет все предыдущие страницы в стеке навигации.|

Кроме того `INavigationService` указывает интерфейс, должен предоставить класс, реализующий `PreviousPageViewModel` свойство. Это свойство возвращает тип модели представления, связанные с предыдущей страницы в стеке навигации.

> [!NOTE]
> `INavigationService` Интерфейс обычно также указать `GoBackAsync` метод, который используется для возврата на предыдущую страницу в стеке навигации программными средствами. Однако этот метод отсутствует в мобильном приложении eShopOnContainers, так как это не обязательно.

### <a name="creating-the-navigationservice-instance"></a>Создание экземпляра NavigationService

`NavigationService` Класса, который реализует `INavigationService` интерфейсом, зарегистрированные как Singleton-класс с контейнер внедрения зависимостей Autofac, как показано в следующем примере кода:

```csharp
builder.RegisterType<NavigationService>().As<INavigationService>().SingleInstance();
```

`INavigationService` Интерфейс разрешается в `ViewModelBase` конструктора класса, как показано в следующем примере кода:

```csharp
NavigationService = ViewModelLocator.Resolve<INavigationService>();
```

Возвращает ссылку на `NavigationService` объект, хранящийся в Autofac контейнер внедрения зависимостей, который создается с `InitNavigation` метод `App` класса. Дополнительные сведения см. в разделе [перехода при запуске приложения](#navigating_when_the_app_is_launched).

`ViewModelBase` Класса хранилищ `NavigationService` экземпляра в `NavigationService` свойство типа `INavigationService`. Таким образом, все представлений классы модели, которые являются производными от `ViewModelBase` класса, можно использовать `NavigationService` свойство для доступа к методам, определяемое `INavigationService` интерфейса. Это позволяет избежать издержек по вводится `NavigationService` объекта из контейнера внедрения зависимостей Autofac в каждый класс модели представления.

### <a name="handling-navigation-requests"></a>Обработка запросов навигации

Xamarin.Forms предоставляет [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) класс, который обеспечивает взаимодействие иерархическая навигация, в которой пользователь имеет возможность перемещаться по страницам, вперед и назад в случае необходимости. Дополнительные сведения об иерархической навигации см. в статье [Иерархическая навигация](~/xamarin-forms/app-fundamentals/navigation/hierarchical.md).

Вместо использования [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) напрямую, класс формирует оболочку для приложения eShopOnContainers `NavigationPage` класса в `CustomNavigationView` класса, как показано в следующем примере кода:

```csharp
public partial class CustomNavigationView : NavigationPage  
{  
    public CustomNavigationView() : base()  
    {  
        InitializeComponent();  
    }  

    public CustomNavigationView(Page root) : base(root)  
    {  
        InitializeComponent();  
    }  
}
```

Данную оболочку предназначено для упрощения стили [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) экземпляр в файле XAML для класса.

Навигации выполняется внутри классов модели представления путем вызова одного из `NavigateToAsync` методы, указав тип модели представления для страницы, куда выполняется переход к, как показано в следующем примере кода:

```csharp
await NavigationService.NavigateToAsync<MainViewModel>();
```

В следующем примере кода показан `NavigateToAsync` методы, предоставляемые `NavigationService` класса:

```csharp
public Task NavigateToAsync<TViewModel>() where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), null);  
}  

public Task NavigateToAsync<TViewModel>(object parameter) where TViewModel : ViewModelBase  
{  
    return InternalNavigateToAsync(typeof(TViewModel), parameter);  
}
```

Каждый метод позволяет любой класс модели представления, производный от `ViewModelBase` классом для выполнения иерархическая навигация с помощью вызова `InternalNavigateToAsync` метод. Кроме того второй `NavigateToAsync` метод, который позволяет навигации данных определен как аргумент, передаваемый модель представления, куда выполняется переход, где он обычно используется для выполнения инициализации. Дополнительные сведения см. в разделе [передача параметров во время навигации](#passing_parameters_during_navigation).

`InternalNavigateToAsync` Метод выполняет запрос навигации и показано в следующем примере кода:

```csharp
private async Task InternalNavigateToAsync(Type viewModelType, object parameter)  
{  
    Page page = CreatePage(viewModelType, parameter);  

    if (page is LoginView)  
    {  
        Application.Current.MainPage = new CustomNavigationView(page);  
    }  
    else  
    {  
        var navigationPage = Application.Current.MainPage as CustomNavigationView;  
        if (navigationPage != null)  
        {  
            await navigationPage.PushAsync(page);  
        }  
        else  
        {  
            Application.Current.MainPage = new CustomNavigationView(page);  
        }  
    }  

    await (page.BindingContext as ViewModelBase).InitializeAsync(parameter);  
}  

private Type GetPageTypeForViewModel(Type viewModelType)  
{  
    var viewName = viewModelType.FullName.Replace("Model", string.Empty);  
    var viewModelAssemblyName = viewModelType.GetTypeInfo().Assembly.FullName;  
    var viewAssemblyName = string.Format(  
                CultureInfo.InvariantCulture, "{0}, {1}", viewName, viewModelAssemblyName);  
    var viewType = Type.GetType(viewAssemblyName);  
    return viewType;  
}  

private Page CreatePage(Type viewModelType, object parameter)  
{  
    Type pageType = GetPageTypeForViewModel(viewModelType);  
    if (pageType == null)  
    {  
        throw new Exception($"Cannot locate page type for {viewModelType}");  
    }  

    Page page = Activator.CreateInstance(pageType) as Page;  
    return page;  
}
```

`InternalNavigateToAsync` Метод выполняет переход к модели представления, сначала вызывается метод `CreatePage` метод. Этот метод осуществляет поиск представление, соответствующий типу модели указанное представление и создает и возвращает экземпляр данного типа представления. Расположение представления, соответствующий типу модели представления используется подход на основе соглашений, в который предполагается, что:

-   Представления находятся в той же сборке, как типы модели представления.
-   Представления находятся в. Дочернее пространство имен представления.
-   Просмотр моделей находятся в. Дочернее пространство имен ViewModels.
-   Для просмотра имен модели, с «Модель» удалены соответствуют имена представлений.

При создании экземпляра представления, она связывается с его соответствующей модели представления. Дополнительные сведения о том, как это происходит, см. в разделе [автоматическое создание модели представления с локатора модели представления](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

Если создается представление `LoginView`, оно помещается в новом экземпляре `CustomNavigationView` класса и назначены [ `Application.Current.MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) свойство. В противном случае `CustomNavigationView` получить экземпляр и указано, что он не имеет значение null, [ `PushAsync` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) метод вызывается для принудительной отправки представление создается стек навигации. Тем не менее если полученный `CustomNavigationView` экземпляр `null`, создаваемого представления упакован в новый экземпляр `CustomNavigationView` класса и назначены `Application.Current.MainPage` свойство. Этот механизм гарантирует, что во время перехода, добавляются страницы правильно в стеке навигации при пустом и если она содержит данные.

> [!TIP]
> Рассмотрите возможность кэширования страниц. Кэширование страниц приводит потребление памяти для представления, которые в настоящий момент не отображаются. Тем не менее без кэширования страницы это означает, что анализ XAML и создания страниц и его представление модели будет выполняться при каждом новую страницу осуществляется переход, который может отрицательно повлиять на производительность для сложных страницы. Для хорошо спроектированного страницы, не используйте слишком много элементов управления производительность будет достаточно. Тем не менее кэширование страницы может помочь при обнаружении время низкая скорость загрузки.

После создания представления и осуществлен переход, `InitializeAsync` метода представления связанного представления модели. Дополнительные сведения см. в разделе [передача параметров во время навигации](#passing_parameters_during_navigation).

<a name="navigating_when_the_app_is_launched" />

### <a name="navigating-when-the-app-is-launched"></a>Навигация по приложения при запуске

При запуске приложения `InitNavigation` метод `App` вызывается класс. Этот метод показан в следующем примере кода:

```csharp
private Task InitNavigation()  
{  
    var navigationService = ViewModelLocator.Resolve<INavigationService>();  
    return navigationService.InitializeAsync();  
}
```

Метод создает новый `NavigationService` объекта в контейнер внедрения зависимостей Autofac и возвращает ссылку на него, перед вызовом его `InitializeAsync` метод.

> [!NOTE]
> Когда `INavigationService` решается с помощью интерфейса `ViewModelBase` класса контейнера возвращает ссылку на `NavigationService` объект, который был создан при вызове метода InitNavigation.

В следующем примере кода показан `NavigationService` `InitializeAsync` метод:

```csharp
public Task InitializeAsync()  
{  
    if (string.IsNullOrEmpty(Settings.AuthAccessToken))  
        return NavigateToAsync<LoginViewModel>();  
    else  
        return NavigateToAsync<MainViewModel>();  
}
```

`MainView` Выполняется переход к, если приложение имеет доступ кэшированного маркера, которое используется для проверки подлинности. В противном случае `LoginView` осуществляется переход.

Дополнительные сведения о контейнер внедрения зависимостей Autofac см. в разделе [введение в внедрения зависимостей](~/xamarin-forms/enterprise-application-patterns/dependency-injection.md#introduction_to_dependency_injection).

<a name="passing_parameters_during_navigation" />

### <a name="passing-parameters-during-navigation"></a>Передача параметров во время навигации

Один из `NavigateToAsync` методов, указанных в `INavigationService` интерфейс включает навигации данных определен как аргумент, передаваемый модель представления, куда выполняется переход, где он обычно используется для выполнения инициализации.

Например `ProfileViewModel` класс содержит `OrderDetailCommand` , который выполняется, когда пользователь выбирает заказ на `ProfileView` страницы. В свою очередь, это выполняет `OrderDetailAsync` метод, как показано в следующем примере кода:

```csharp
private async Task OrderDetailAsync(Order order)  
{  
    await NavigationService.NavigateToAsync<OrderDetailViewModel>(order);  
}
```

Этот метод вызывает переход к `OrderDetailViewModel`, передав `Order` экземпляр, который представляет порядок, в котором пользователь выбрал на `ProfileView` страницы. Когда `NavigationService` класс создает `OrderDetailView`, `OrderDetailViewModel` класса создается и назначен представлении [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/). После перехода по `OrderDetailView`, `InternalNavigateToAsync` выполняет метод `InitializeAsync` метод представления, связанного с модели представления.

`InitializeAsync` Метод определяется в `ViewModelBase` класса, что и метод, который может быть переопределен. Этот метод задает `object` аргумент, который представляет данные, передаваемые модель представлений во время операции навигации. Таким образом, представление классов модели, которые требуется получать данные из операции навигации предоставить свою собственную реализацию `InitializeAsync` метод для выполнения инициализации. В следующем примере кода показан `InitializeAsync` метод `OrderDetailViewModel` класса:

```csharp
public override async Task InitializeAsync(object navigationData)  
{  
    if (navigationData is Order)  
    {  
        ...  
        Order = await _ordersService.GetOrderAsync(  
                        Convert.ToInt32(order.OrderNumber), authToken);  
        ...  
    }  
}
```

Этот метод получает `Order` экземпляра, который был передан в модель представления во время операции навигации и использует его для получения полного порядок сведений из `OrderService` экземпляра.

<a name="invoking_navigation_using_behaviors" />

### <a name="invoking-navigation-using-behaviors"></a>Вызов навигации с помощью поведений

Навигации обычно инициируется из представления взаимодействия с пользователем. Например `LoginView` выполняет навигации, после успешной проверки подлинности. В следующем примере кода показано, как навигация вызывается при помощи поведения.

```xaml
<WebView ...>  
    <WebView.Behaviors>  
        <behaviors:EventToCommandBehavior  
            EventName="Navigating"  
            EventArgsConverter="{StaticResource WebNavigatingEventArgsConverter}"  
            Command="{Binding NavigateCommand}" />  
    </WebView.Behaviors>  
</WebView>
```

Во время выполнения `EventToCommandBehavior` будет отвечать на действия с [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/). Когда `WebView` переходит на веб-страницу [ `Navigating` ](https://developer.xamarin.com/api/event/Xamarin.Forms.WebView.Navigating/) активизируется событие, в которой будет выполнен `NavigateCommand` в `LoginViewModel`. По умолчанию аргументы событий для события передаются в команду. Эти данные преобразуются при передаче между источником и целью преобразователем, указанный в `EventArgsConverter` свойство, которое возвращает [ `Url` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebNavigationEventArgs.Url/) из [ `WebNavigatingEventArgs` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebNavigatingEventArgs/). Таким образом, когда `NavigationCommand` будет выполнен, Url-адрес веб-страницы, передается как параметр зарегистрированного `Action`.

В свою очередь `NavigationCommand` выполняет `NavigateAsync` метод, как показано в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...          
    await NavigationService.NavigateToAsync<MainViewModel>();  
    await NavigationService.RemoveLastFromBackStackAsync();  
    ...  
}
```

Этот метод вызывает переход к `MainViewModel`, и следующие навигации, удаляет `LoginView` страницы из стека навигации.

### <a name="confirming-or-cancelling-navigation"></a>Подтверждения или отмены навигации

Для взаимодействия с пользователем во время операции навигации, чтобы пользователь мог подтвердить или отменить перемещение могут понадобиться приложению. Это может потребоваться, например, когда пользователь пытается перейти перед полного завершения страницы записи данных. В этом случае приложение необходимо предоставить уведомление, которое позволяет пользователю покинуть страницу или отменить операцию навигации перед его выполнением. Этого можно достичь в классе представления модели, используя ответа для уведомления для управления, вызывается ли навигации.

## <a name="summary"></a>Сводка

Xamarin.Forms поддерживает навигацию по страницам, что обычно приводит к из взаимодействие пользователя с помощью пользовательского интерфейса или из самого приложения, в результате изменения внутреннего состояния на основе логики. Тем не менее может быть сложно реализовать в приложениях, использующих шаблон MVVM навигации.

В этой главе представлены `NavigationService` класс, который используется для выполнения сначала модель навигации представление из представления модели. Размещения логики навигации в представлении классов модели означает, что через автоматических тестов можно использовать логику. Кроме того модель представления можно затем реализовать логику для управления навигации, чтобы убедиться, что применяются определенные бизнес-правила.


## <a name="related-links"></a>Связанные ссылки

- [Загрузить электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
