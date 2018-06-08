---
title: Жизненный цикл приложения
description: Как реагировать на жизненного цикла приложения
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/31/2018
ms.openlocfilehash: a22ad8f3f272212f5c7f088ba2112f2771ff4a7f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846348"
---
# <a name="app-lifecycle"></a>Жизненный цикл приложения

[ `Application` ](xref:Xamarin.Forms.Application) Базовый класс предоставляет следующие возможности:

* [Методы жизненного цикла](#Lifecycle_Methods) `OnStart`, `OnSleep`, и `OnResume`.
* [События навигации страницы](#page) [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing), [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing).
* [События переходов модального](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, и `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Методы жизненного цикла

[ `Application` ](xref:Xamarin.Forms.Application) Класс содержит три виртуальные методы, которые могут быть переопределены для обработки методов жизненного цикла:

* **OnStart** -вызывается при запуске приложения.

* **OnSleep** -вызывается каждый раз, приложение выходит в фоновом режиме.

* **OnResume** — вызывается, когда приложение возобновляется после отправки в фоновом режиме.

Следует отметить, что *не* метод для завершения приложения.
В обычных условиях (т. е. не сбоя) завершение приложения будет происходить от *OnSleep* состояния без дополнительных уведомления в код.

Чтобы увидеть, когда эти методы вызываются, реализовать `WriteLine` вызов в каждом (как показано ниже) и тестирование на каждой платформе.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

При обновлении *старые* Xamarin.Forms приложений (например) необходимо создать с помощью Xamarin.Forms 1.3 или более ранних версий), убедитесь, что включает Android основных действий `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` в `[Activity()]` атрибута. Если это не представлен будет контролироваться `OnStart` метод вызывается для поворота, а также при первом запуске приложения. Этот атрибут автоматически включается в текущее приложение шаблонами Xamarin.Forms.

<a name="page" />

## <a name="page-navigation-events"></a>Навигация по страницам события

Имеются два события на [ `Application` ](xref:Xamarin.Forms.Application) класс, который уведомлений о страницах, отображаются и исчезновение:

- [`PageAppearing`](xref:Xamarin.Forms.Application.PageAppearing) — вызывается, когда страница — на экране.
- [`PageDisappearing`](xref:Xamarin.Forms.Application.PageDisappearing) — вызывается, когда страница — исчезают с экрана.

Эти события можно использовать в сценариях, где требуется отслеживать страницы, как они появляются на экране.

> [!NOTE]
> [ `PageAppearing` ](xref:Xamarin.Forms.Application.PageAppearing) И [ `PageDisappearing` ](xref:Xamarin.Forms.Application.PageDisappearing) событий из [ `Page` ](xref:Xamarin.Forms.Page) базового класса сразу после [ `Page.Appearing` ](xref:Xamarin.Forms.Page.Appearing) и [ `Page.Disappearing` ](xref:Xamarin.Forms.Page.Disappearing) события, соответственно.

<a name="modal" />

## <a name="modal-navigation-events"></a>События модальное навигации

Существует четыре события на [ `Application` ](xref:Xamarin.Forms.Application) классов, каждый из которых собственные аргументы события, которые позволяют реагировать на модальное страницы отображаются и закрывается:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` класс содержит `Cancel` свойство. Когда `Cancel` равно `true` модальное pop отменяется.
* **ModalPopped** - `ModalPoppedEventArgs`

> [!NOTE]
> Для реализации методов жизненного цикла приложения и события модальное навигации, все предварительной`Application` способы создания приложения Xamarin.Forms (т. е. приложения, написанные в версии 1.2 или более ранних версий, используйте статический `GetMainPage` метод) были обновлены, чтобы создать по умолчанию `Application` , установленного в качестве родительского для `MainPage`.
>
> Xamarin.Forms приложений, использующих к поведению предыдущих версий необходимо обновить до `Application` подкласс, как описано в статье [класс приложения](~/xamarin-forms/app-fundamentals/application-class.md) страницы.
