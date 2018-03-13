---
title: "Индексирование приложения и глубокое связывание"
description: "Индексирование приложения позволяет приложениям, в противном случае — забывается после использует несколько оставаться применимо, в результатах поиска. Глубокое связывание позволяет приложениям реагировать на результат поиска, который содержит данные приложений, как правило, перейдя на страницу, на которые ссылается прямая ссылка. В этой статье показано, как использовать приложение индексирования и глубокое связывание для предоставления доступа к содержимому приложения Xamarin.Forms для поиска на устройствах iOS и Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 410C5D19-AA3C-4E0D-B799-E288C5803226
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 07/11/2016
ms.openlocfilehash: 38d3b6da0dd33e038f2d50209280f2983faf6013
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="application-indexing-and-deep-linking"></a>Индексирование приложения и глубокое связывание

_Индексирование приложения позволяет приложениям, в противном случае — забывается после использует несколько оставаться применимо, в результатах поиска. Глубокое связывание позволяет приложениям реагировать на результат поиска, который содержит данные приложений, как правило, перейдя на страницу, на которые ссылается прямая ссылка. В этой статье показано, как использовать приложение индексирования и глубокое связывание для предоставления доступа к содержимому приложения Xamarin.Forms для поиска на устройствах iOS и Android._

> [!VIDEO https://youtube.com/embed/UJv4jUs7cJw]

**Глубокие связывания с Xamarin.Forms и Azure, с [университета Xamarin](https://university.xamarin.com/)**


Индексирование Xamarin.Forms приложения и глубокое связывание предоставляют API для публикации метаданных для приложения индексирования, как пользователи перемещаются между приложениями. Затем можно искать индексированное содержимое, в поиску Spotlight, Google поиск или поиск в Интернете. Нажав на результат поиска, который содержит прямая ссылка будет срабатывать событие, которое может обрабатываться с помощью приложения и обычно используется для перехода к странице, на которые ссылается прямая ссылка.

Образец приложения иллюстрирует приложения списка задач, где данные хранятся в локальной базе данных SQLite, как показано на следующем снимке экрана:

![](deep-linking-images/screenshots.png "Приложения TodoList")

Каждый `TodoItem` индексируется экземпляра, созданных пользователем. Специфический для платформы поиска можно найти индексированных данных из приложения. Когда пользователь нажимает на элемент в результатах поиска для приложения, приложение запускается, `TodoItemPage` осуществляется переход и `TodoItem` ссылки из сложного отображается ссылка.

Дополнительные сведения об использовании базы данных SQLite см. в разделе [работа с локальной базы данных](~/xamarin-forms/app-fundamentals/databases.md).

> [!NOTE]
> Xamarin.Forms приложения индексирования и сложного связывания функциональность доступна только для iOS и Android платформ и требует iOS 9 и API 23 соответственно.

## <a name="setup"></a>Установка

Любые дополнительные инструкции по установке с помощью этой функции в iOS и Android платформы в следующих разделах.

### <a name="ios"></a>iOS

На платформе iOS — без дополнительной настройки, необходимые для применения этих функциональных возможностей.

### <a name="android"></a>Android

На платформе Android существует несколько предварительных требований, которые должны быть выполнены для использования приложения индексирования и сложного связывания функциональные возможности:

1. Версия приложения должна находиться в Google Play.
1. Помощник по поиску веб-сайт должен быть зарегистрирован для приложения в консоли разработчика Google. Как только приложение связано с веб-сайта, URL-адреса могут быть проиндексированы, работающих веб-сайт и приложение, которое затем может быть предоставлен в результатах поиска. Дополнительные сведения см. в разделе [приложения индексирования поиска Google](https://support.google.com/googleplay/android-developer/answer/6041489) на веб-сайте Google.
1. Приложение должно поддерживать целей URL-адрес HTTP на `MainActivity` класс, который сообщить индексирования какие типы данных схем URL-адрес приложения, приложение может реагировать на. Дополнительные сведения см. в разделе [Настройка фильтра намерение](~/android/platform/app-linking.md#configure-intent-filter).

Если эти условия выполнены, для использования индексирования приложения Xamarin.Forms и глубокое связывание на платформе Android требуются следующие дополнительные:

1. Установка [Xamarin.Forms.AppLinks](https://www.nuget.org/packages/Xamarin.Forms.AppLinks/) пакета NuGet в проект приложения Android.
1. В `MainActivity.cs` файл, импортируйте `Xamarin.Forms.Platform.Android.AppLinks` пространства имен.
1. В `MainActivity.OnCreate` переопределения, добавьте следующий код под `Forms.Init(this, bundle)`:

```csharp
AndroidAppLinks.Init (this);
```

Дополнительные сведения см. в разделе [глубокой ссылку содержимого с помощью Xamarin.Forms URL навигации](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) в блоге Xamarin.

## <a name="indexing-a-page"></a>Индексации страницы

Процесс индексации страницы и предоставляя его поиск в Google и Spotlight выглядит следующим образом:

1. Создание [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) , содержащий метаданные, необходимые для индексирования страницы, а также прямую ссылку на вернуться на страницу, когда пользователь выбирает индексированное содержимое в результатах поиска.
1. Зарегистрировать [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляра для индексирования его для поиска.

В следующем примере кода показано, как создать [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляр:

```csharp
AppLinkEntry GetAppLink (TodoItem item)
{
  var pageType = GetType ().ToString ();
  var pageLink = new AppLinkEntry {
    Title = item.Name,
    Description = item.Notes,
    AppLinkUri = new Uri (string.Format ("http://{0}/{1}?id={2}",
      App.AppName, pageType, WebUtility.UrlEncode (item.ID)), UriKind.RelativeOrAbsolute),
    IsLinkActive = true,
    Thumbnail = ImageSource.FromFile ("monkey.png")
  };

  return pageLink;
}
```

[ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) Экземпляр содержит ряд свойств, значения которых необходимы для индекса страницы и создать прямая ссылка. [ `Title` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Title/), [ `Description` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Description/), И [ `Thumbnail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.Thumbnail/) свойства используются для определения индекса содержимого, когда он появится в результатах поиска. [ `IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) Свойству `true` для указания, что индексированное содержимое в настоящий момент просматривается. [ `AppLinkUri` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.AppLinkUri/) Свойство `Uri` , содержащий сведения, необходимые для возврата к текущей странице и отображение текущей `TodoItem`. В следующем примере показано пример `Uri` для примера приложения:

```csharp
http://deeplinking/DeepLinking.TodoItemPage?id=ec38ebd1-811e-4809-8a55-0d028fce7819
```

Это `Uri` содержит все сведения, необходимые для запуска `deeplinking` приложения, перейдите к `DeepLinking.TodoItemPage`и отобразить `TodoItem` с `ID` из `ec38ebd1-811e-4809-8a55-0d028fce7819`.

## <a name="registering-content-for-indexing"></a>Регистрация для индексирования содержимого

Один раз [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) был создан экземпляр, он должен быть зарегистрирован для индексирования в результатах поиска. Это осуществляется с помощью [ `RegisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.RegisterLink/p/Xamarin.Forms.IAppLinkEntry/) метода, как показано в следующем примере кода:

```csharp
Application.Current.AppLinks.RegisterLink (appLink);
```

Это добавляет [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляра в приложение [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) коллекции.

> [!NOTE]
> `RegisterLink` Метод может также использоваться для обновления содержимого, которая индексируется для страницы.

Один раз [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляр был зарегистрирован для индексирования, он может отображаться в результатах поиска. На следующем рисунке показан индексированное содержимое, в результатах поиска на платформе iOS:

![](deep-linking-images/ios-search.png "Индексированное содержимое в результатах поиска в iOS")

## <a name="de-registering-indexed-content"></a>Отмена регистрации индексированное содержимое

[ `DeregisterLink` ](https://developer.xamarin.com/api/member/Xamarin.Forms.IAppLinks.DeregisterLink/p/Xamarin.Forms.IAppLinkEntry/) Метод используется для удаления индексированное содержимое из результатов поиска, как показано в следующем примере кода:

```csharp
Application.Current.AppLinks.DeregisterLink (appLink);
```

Эта функция удаляет [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляра из приложения [ `AppLinks` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.AppLinks/) коллекции.

> [!NOTE]
> В Android не удается удалить индексированное содержимое из результатов поиска.

<a name="responding" />

## <a name="responding-to-a-deep-link"></a>Реакция на прямая ссылка

Если индексированное содержимое отображается в результатах поиска и выбора пользователем, `App` класс приложение будет получать запрос для обработки `Uri` содержащихся в индекс содержимого. Этот запрос может быть обработано в [ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) переопределить, как показано в следующем примере кода:

```csharp
public class App : Application
{
  ...

  protected override async void OnAppLinkRequestReceived (Uri uri)
  {
    string appDomain = "http://" + App.AppName.ToLowerInvariant () + "/";
    if (!uri.ToString ().ToLowerInvariant ().StartsWith (appDomain)) {
      return;
    }

    string pageUrl = uri.ToString ().Replace (appDomain, string.Empty).Trim ();
    var parts = pageUrl.Split ('?');
    string page = parts [0];
    string pageParameter = parts [1].Replace ("id=", string.Empty);

    var formsPage = Activator.CreateInstance (Type.GetType (page));
    var todoItemPage = formsPage as TodoItemPage;
    if (todoItemPage != null) {
      var todoItem = App.Database.Find (pageParameter);
      todoItemPage.BindingContext = todoItem;
      await MainPage.Navigation.PushAsync (formsPage as Page);
    }

    base.OnAppLinkRequestReceived (uri);
  }
}
```

[ `OnAppLinkRequestReceived` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Application.OnAppLinkRequestReceived/p/System.Uri/) Метод проверяет, что полученных `Uri` предназначен для приложения перед синтаксическим анализом `Uri` на страницу, чтобы осуществить переход и параметр для передачи на страницу. Экземпляр страницы, чтобы рассматриваться для создания и `TodoItem` представлены на странице параметр получается. [ `BindingContext` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BindableObject.BindingContext/) Страницы, чтобы осуществить переход присваивается значение `TodoItem`. Это гарантирует, что при `TodoItemPage` отображается по [ `PushAsync` ](https://developer.xamarin.com/api/member/Xamarin.Forms.INavigation.PushAsync/p/Xamarin.Forms.Page/) метод, он будет отображаться `TodoItem` которого `ID` содержится в прямая ссылка.

## <a name="making-content-available-for-search-indexing"></a>Предоставление содержимого для индексирования поиска

Каждый раз, отображается страница, представленный прямая ссылка, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) свойству можно присвоить значение `true`. В iOS и Android в результате [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляра для индексирования поиска и на только iOS, таким образом, `AppLinkEntry` экземпляра, доступных для обработки. Дополнительные сведения о перемещение вручную см. в разделе [Общие сведения о переадресации](~/ios/platform/handoff.md).

В следующем примере кода демонстрируется настройка [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) свойства `true` в [ `Page.OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) переопределения:

```csharp
protected override void OnAppearing ()
{
  appLink = GetAppLink (BindingContext as TodoItem);
  if (appLink != null) {
    appLink.IsLinkActive = true;
  }
}
```

Аналогичным образом, когда страницы, представленной прямая ссылка будет выполнен переход от, [ `AppLinkEntry.IsLinkActive` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.IsLinkActive/) свойству можно присвоить значение `false`. В iOS и Android, при этом прекращается выполнение [ `AppLinkEntry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/) экземпляра, объявленные для индексирования поиска, а также на iOS только, она также останавливает рекламных `AppLinkEntry` экземпляр для обработки. Это можно сделать в [ `Page.OnDisappearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnDisappearing()/) переопределить, как показано в следующем примере кода:

```csharp
protected override void OnDisappearing ()
{
  if (appLink != null) {
    appLink.IsLinkActive = false;
  }
}
```

## <a name="providing-data-to-handoff"></a>Предоставление данных для передачи

На iOS относящиеся к приложению данные могут храниться при индексации страницы. Это достигается путем добавления данных к [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) коллекции, являющийся `Dictionary<string, string>` для хранения пар "ключ значение", используемые в перемещение вручную. Перемещение вручную — это способ для пользователя для запуска действия на одном из своих устройств и продолжить это действие на другом свои устройства (как определено в учетной записи пользователя iCloud). Ниже показан пример хранения пар ключ значение для конкретного приложения.

```csharp
var pageLink = new AppLinkEntry {
  ...  
};
pageLink.KeyValues.Add("appName", App.AppName);
pageLink.KeyValues.Add("companyName", "Xamarin");
```

Значения, хранящиеся в [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) коллекции будут храниться в метаданных для индексированного страницы и будут восстановлены, когда пользователь нажимает на результат поиска, содержащий прямая ссылка (или при использовании перемещение вручную для просмотра содержимого на другом вход в устройство).

Кроме того можно указать значения для следующих разделов:

- `contentType` — `string` , задающий идентификатор универсальный тип индекса содержимого. Рекомендуемое соглашение для этого значения — это имя типа страницы, содержащей индексированное содержимое.
- `associatedWebPage` — `string` , представляющий посетите Если индексированное содержимое также можно просмотреть в Интернете, или если приложение поддерживает Safari прямые ссылки на веб-странице.
- `shouldAddToPublicIndex` — `string` любого `true` или `false` , управляет ли добавить индексированное содержимое индекса общедоступном облаке компании Apple, который затем могут быть представлены пользователям, которые еще не установили приложение на устройстве iOS. Однако только потому, что содержимое задана для открытого индексирования, это не значит, что он автоматически добавляется к индексу общедоступном облаке компании Apple. Дополнительные сведения см. в разделе [открытый индексирования поиска](~/ios/platform/search/nsuseractivity.md). Обратите внимание, что этот ключ должно быть присвоено `false` при добавлении личных данных [ `KeyValues` ](https://developer.xamarin.com/api/property/Xamarin.Forms.IAppLinkEntry.KeyValues/) коллекции.

> [!NOTE]
> `KeyValues` Коллекции не используется на платформе Android.

Дополнительные сведения о перемещение вручную см. в разделе [Общие сведения о переадресации](~/ios/platform/handoff.md).

## <a name="summary"></a>Сводка

В этой статье показано, как использовать приложение индексирования и глубокое связывание для предоставления доступа к содержимому приложения Xamarin.Forms для поиска на устройствах iOS и Android. Индексирование приложения позволяет приложениям Оставайтесь применимо, в результатах поиска, которые бы в противном случае — забывается о после использует несколько.


## <a name="related-links"></a>Связанные ссылки

- [Глубокие компоновка (пример)](https://developer.xamarin.com/samples/xamarin-forms/deeplinking/)
- [API поиска iOS](~/ios/platform/search/index.md)
- [Связывание приложения в Android 6.0](~/android/platform/app-linking.md)
- [AppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.AppLinkEntry/)
- [IAppLinkEntry](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinkEntry/)
- [IAppLinks](https://developer.xamarin.com/api/type/Xamarin.Forms.IAppLinks/)
