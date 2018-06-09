---
title: Управление конфигурацией
description: В этой главе объясняется, как eShopOnContainers мобильное приложение реализует управление конфигурацией для предоставления параметров приложения и параметры пользователя.
ms.prod: xamarin
ms.assetid: 50d6e780-e768-47f8-9361-3af11e56b87b
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/07/2017
ms.openlocfilehash: d6cd9771760bc2932345fec24887842ce1c47376
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243955"
---
# <a name="configuration-management"></a>Управление конфигурацией

Параметры позволяют разделение данных, используемый для настройки поведения приложения, от кода, позволяя поведение, необходимо изменить без повторного построения приложения. Существует два типа параметров: параметры приложения и параметры пользователя.

Параметры приложений — это данные, приложение создает и управляет ими. Он может включать данные, например основных сетевые конечные точки службы, ключи API и состояние выполнения. Параметры приложения связаны с существования приложения и важны только для этого приложения.

Параметры пользователя являются настраиваемых параметров приложения, которые влияют на поведение приложения и не требуют частого повторной настройки. Например приложение может быть реализована возможность указать источник для получения данных из и способ их отображения на экране.

Xamarin.Forms включает постоянное словарь, который может использоваться для хранения данных параметров. Этот словарь можно осуществлять с помощью [ `Application.Current.Properties` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.Properties/) свойство и все данные, помещается в него сохраняется при переходит в спящий режим и приложения будут восстановлены при запуске или возобновляет работу приложения. Кроме того [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) класс также содержит [ `SavePropertiesAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.SavePropertiesAsync()/) метод, который позволяет приложению, его параметры сохраняются при необходимости. Дополнительные сведения об этом словаре см. в разделе [словарь свойств](~/xamarin-forms/app-fundamentals/application-class.md#Properties_Dictionary).

Сохранение данных с помощью постоянных словарь Xamarin.Forms недостатком является то, что это не просто данными, привязанными к. Таким образом, eShopOnContainers мобильное приложение использует библиотеку Xam.Plugins.Settings из [NuGet](https://www.nuget.org/packages/Xam.Plugins.Settings/). Эта библиотека предоставляет согласованное, строго типизированным, кросс платформенных подход для сохранения и извлечения параметров приложения и пользователей, при использовании собственного параметры управления, предоставляемые каждой платформы. Кроме того нетрудно использовать привязку данных для доступа к данным параметры, предоставляемые библиотекой.

> [!NOTE]
> Несмотря на то, библиотека Xam.Plugin.Settings могут сохранять приложения и пользовательские параметры, становится нет различий между ними.

## <a name="creating-a-settings-class"></a>Создание класса параметров

При использовании библиотеки Xam.Plugins.Settings один статический класс должен быть создан, будет содержать приложения и пользовательские параметры, необходимые для приложения. В следующем примере кода показан класс параметры в мобильном приложении eShopOnContainers:

```csharp
public static class Settings  
{  
    private static ISettings AppSettings  
    {  
        get  
        {  
            return CrossSettings.Current;  
        }  
    }  
    ...  
}
```

Параметры можно считывать и записывать новые `ISettings` API, предоставляемый библиотеке Xam.Plugins.Settings. Эта библиотека предоставляет одноэлементным. он может использоваться для доступа к API, `CrossSettings.Current`, и класс параметров приложения должны предоставлять этом единственном экземпляре через `ISettings` свойство.

> [!NOTE]
> Директивы using для пространства имен Plugin.Settings и Plugin.Settings.Abstractions должны быть добавлены в класс, необходим доступ к Xam.Plugins.Settings библиотеки типов.

## <a name="adding-a-setting"></a>Добавление параметра

Каждый параметр состоит из ключа, значение по умолчанию и свойства. В следующем примере кода показаны все три элемента для параметра пользователя, который представляет базовый URL-адрес для веб-служб, eShopOnContainers мобильное приложение подключается к:

```csharp
public static class Settings  
{  
    ...  
    private const string IdUrlBase = "url_base";  
    private static readonly string UrlBaseDefault = GlobalSetting.Instance.BaseEndpoint;  
    ...  

    public static string UrlBase  
    {  
        get  
        {  
            return AppSettings.GetValueOrDefault<string>(IdUrlBase, UrlBaseDefault);  
        }  
        set  
        {  
            AppSettings.AddOrUpdateValue<string>(IdUrlBase, value);  
        }  
    }  
}
```

Ключ является всегда const строка, определяющая имя ключа, со значением по умолчанию для параметра статического доступного только для чтения значение требуемого типа. Возвращает значение по умолчанию обеспечивает допустимое значение доступной, если не задано параметром извлекается.

`UrlBase` Статическое свойство используются два метода из `ISettings` API для чтения или записи значение параметра. `ISettings.GetValueOrDefault` Метод используется для получения значения параметра из хранилища конкретную платформу. Если значение не определено для параметра, вместо извлечения его значения по умолчанию. Аналогичным образом `ISettings.AddOrUpdateValue` метод используется для сохранения значения параметра в хранилище конкретную платформу.

Вместо этого, определяющие значения по умолчанию внутри `Settings` класса, `UrlBaseDefault` строка получает свое значение из `GlobalSetting` класса. В следующем примере кода показан `BaseEndpoint` свойство и `UpdateEndpoint` метода в данном классе:

```csharp
public class GlobalSetting  
{  
    ...  
    public string BaseEndpoint  
    {  
        get { return _baseEndpoint; }  
        set  
        {  
            _baseEndpoint = value;  
            UpdateEndpoint(_baseEndpoint);  
        }  
    }  
    ...  

    private void UpdateEndpoint(string baseEndpoint)  
    {  
        RegisterWebsite = string.Format("{0}:5105/Account/Register", baseEndpoint);  
        CatalogEndpoint = string.Format("{0}:5101", baseEndpoint);  
        OrdersEndpoint = string.Format("{0}:5102", baseEndpoint);  
        BasketEndpoint = string.Format("{0}:5103", baseEndpoint);  
        IdentityEndpoint = string.Format("{0}:5105/connect/authorize", baseEndpoint);  
        UserInfoEndpoint = string.Format("{0}:5105/connect/userinfo", baseEndpoint);  
        TokenEndpoint = string.Format("{0}:5105/connect/token", baseEndpoint);  
        LogoutEndpoint = string.Format("{0}:5105/connect/endsession", baseEndpoint);  
        IdentityCallback = string.Format("{0}:5105/xamarincallback", baseEndpoint);  
        LogoutCallback = string.Format("{0}:5105/Account/Redirecting", baseEndpoint);  
    }  
}
```

Каждый раз `BaseEndpoint` свойство задано, `UpdateEndpoint` вызывается метод. Этот метод обновляет ряд свойств, которые основаны на `UrlBase` значения параметров, предоставляемая пользователем `Settings` класс, представляющий различные конечные точки, которые eShopOnContainers мобильное приложение подключается к.

## <a name="data-binding-to-user-settings"></a>Привязка данных к параметров пользователя

В мобильном приложении eShopOnContainers `SettingsView` предоставляет два параметра для пользователя. Эти параметры позволяют настраивать ли приложение должно получать данные из микрослужбами, который развернут как контейнеры Docker, или ли приложение должно получать данные из макетов служб, которые не требуют подключения к Интернету. При выборе для извлечения данных из контейнерного микрослужбами, необходимо указать URL-адрес конечной точки базовый микрослужбами. На рисунке 7-1 показано `SettingsView` Если пользователь выбрал для извлечения данных из контейнерного микрослужбами.

![](configuration-management-images/settings-endpoint.png "Пользовательские параметры, предоставляемые eShopOnContainers мобильного приложения")

**Рис. 7-1**: пользовательские параметры, предоставляемые eShopOnContainers мобильного приложения

Привязка данных может использоваться для извлечения и установки параметров, предоставляемых `Settings` класса. Это достигается за счет элементов управления на привязку представления, чтобы просмотреть свойства модели, которые в свою очередь доступа к свойствам в `Settings` класса и вызов свойства изменить уведомления при изменении значения параметров. Сведения о как eShopOnContainers мобильное приложение создает представление модели и связывает их с представлениями, см. в разделе [автоматическое создание модели представления с локатора модели представления](~/xamarin-forms/enterprise-application-patterns/mvvm.md#automatically_creating_a_view_model_with_a_view_model_locator).

В следующем примере кода показан [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления из `SettingsView` , позволяющий пользователю вводить URL-адрес конечной точки базовый для контейнерного микрослужбами:

```xaml
<Entry Text="{Binding Endpoint, Mode=TwoWay}" />
```

Это [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) управления привязывается к `Endpoint` свойство `SettingsViewModel` класс с помощью двусторонней привязки. В следующем примере кода показаны свойства конечной точки:

```csharp
public string Endpoint  
{  
    get { return _endpoint; }  
    set  
    {  
        _endpoint = value;  

        if(!string.IsNullOrEmpty(_endpoint))  
        {  
            UpdateEndpoint(_endpoint);  
        }  

        RaisePropertyChanged(() => Endpoint);  
    }  
}
```

Когда `Endpoint` свойству `UpdateEndpoint` вызывается метод, возникает, если предоставленное значение является допустимым, а изменить свойство уведомления. В следующем примере кода показан `UpdateEndpoint` метод:

```csharp
private void UpdateEndpoint(string endpoint)  
{  
    Settings.UrlBase = endpoint;  
}
```

Этот метод обновляет `UrlBase` свойство в `Settings` значение URL-адреса конечной точки базового класса, введенные пользователем, что приведет к сохраняются в хранилище специфический для платформы.

Когда `SettingsView` осуществляется переход, `InitializeAsync` метод `SettingsViewModel` класса выполняется. Этот метод показан в следующем примере кода:

```csharp
public override Task InitializeAsync(object navigationData)  
{  
    ...  
    Endpoint = Settings.UrlBase;  
    ...  
}
```

Задает метод `Endpoint` значение из свойства `UrlBase` свойство в `Settings` класса. Доступ к `UrlBase` свойство вызывает библиотеку Xam.Plugins.Settings для извлечения значения параметров из хранилища конкретную платформу. Сведения о том, как `InitializeAsync` вызывается метод см. в разделе [передача параметров во время навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

Этот механизм гарантирует, что всякий раз, когда пользователь переходит на SettingsView, параметры пользователя, извлекаются из хранилища платформой и представленных через привязку данных. Затем, если пользователь изменяет значения параметров, привязки данных гарантирует, что они немедленно сохраняются в хранилище специфический для платформы.

## <a name="summary"></a>Сводка

Параметры позволяют разделение данных, используемый для настройки поведения приложения, от кода, позволяя поведение, необходимо изменить без повторного построения приложения. Параметры приложений — это данные, приложение создает и управляет ими, и параметров пользователя настраиваемых параметров приложения, которые влияют на поведение приложения и не требуют частого повторной настройки.

Библиотека Xam.Plugins.Settings обеспечивает согласованное, типобезопасный, кросс платформенных подход для сохранения и извлечения приложения и параметров пользователя и привязки данных можно использовать для доступа к параметрам, созданные с помощью библиотеки.


## <a name="related-links"></a>Связанные ссылки

- [Загрузить электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
