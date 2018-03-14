---
title: "Введение в iOS 6"
description: "iOS 6 включает множество новых технологий для разработки приложений, которые Xamarin.iOS 6 предоставляет разработчикам C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 242DA7E3-8FD8-5F20-285D-603259CA622D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: e1232c96f5ea978c8adc640160a1e9b7e42663d8
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="introduction-to-ios-6"></a>Введение в iOS 6

_iOS 6 включает множество новых технологий для разработки приложений, которые Xamarin.iOS 6 предоставляет разработчикам C#._

[ ![](images/ios6-large.jpg "Эмблема iOS 6")](images/ios6-large.jpg#lightbox)

IOS 6 и Xamarin.iOS 6 разработчики теперь имеют широкий набор возможностей в их распоряжении, для создания приложений iOS, включая те, что целевой iPhone 5.
В этом документе приведены более новые функции, доступные и ссылки на статьи, для каждого раздела. Кроме того она затрагивает некоторые изменения, которые будут важны при их перемещении iOS 6 и новое разрешение iPhone 5.


## <a name="introduction-to-collection-viewsiosuser-interfacecontrolsuicollectionviewmd"></a>[Общие сведения о представлениях коллекций](~/ios/user-interface/controls/uicollectionview.md)

Представления коллекций разрешить доступ к содержимому будет отображаться с использованием произвольного макеты. Они позволяют легко компоновке вид сетки без дополнительной настройки, а также поддержку пользовательских макетов также. Дополнительные сведения см., [введение к представлениям коллекции](~/ios/user-interface/controls/uicollectionview.md) [ ](~/ios/user-interface/controls/uicollectionview.md)руководства.


## <a name="introduction-to-pass-kitiosplatformpasskitmd"></a>[Общие сведения для передачи пакета](~/ios/platform/passkit.md)

Платформа передачи пакета позволяет приложениям взаимодействовать с цифровой передает, управление которыми осуществляется в расчетной книжке. Дополнительные сведения см., [введение в руководство по пакету передачи](~/ios/platform/passkit.md).


##  <a name="introduction-to-event-kitiosplatformeventkitmd"></a>[Введение в набор событий](~/ios/platform/eventkit.md)

Платформа EventKit предоставляет способ доступа к календарям, событий календаря и напоминания данные хранятся в базе данных календаря. Доступ к календарям и календарь событий был доступен с iOS 4, но iOS 6 теперь предоставляет доступ к данным напоминания. Дополнительные сведения см. в разделе [я](~/ios/platform/eventkit.md) [ntroduction для EventKit](~/ios/platform/eventkit.md) руководства.


##  <a name="introduction-to-the-social-frameworkiosplatformsocial-frameworkmd"></a>[Знакомство с платформой социальных сетей](~/ios/platform/social-framework.md)

Социальные Framework предоставляет универсальный интерфейс API для взаимодействия с социальными сетями, включая Twitter и Facebook, а также SinaWeibo для пользователей в Китае. Дополнительные сведения см., [Знакомство с платформой социальных](~/ios/platform/social-framework.md) руководства.


##  <a name="changes-to-store-kitchanges-to-storekitmd"></a>[Изменения для хранения пакета](changes-to-storekit.md)

Добавлены две новые функции в набор магазина Apple: приобретение и загрузке iTunes или App Store содержимого из приложения и размещение файлов содержимого для покупки из приложений. Дополнительные сведения см., [примет набор магазина](changes-to-storekit.md) руководства.


## <a name="other-changes"></a>Другие изменения


### <a name="viewwillunload-and-viewdidunload-deprecated"></a>ViewWillUnload и ViewDidUnload рекомендуется к использованию

`ViewWillUnload` И `ViewDidUnload` методы `UIViewController` больше не вызываются в iOS 6. В предыдущих версиях операций ввода-вывода эти методы использовался приложениями для сохранения состояния перед выгружает представления и кода очистки, соответственно.

Например, Visual Studio для Mac создать метод с именем `ReleaseDesignerOutlets`, показанный ниже, который будет вызываться из `ViewDidUnload`:

```csharp
void ReleaseDesignerOutlets ()
{
    if (myOutlet != null) {
        myOutlet.Dispose ();
        myOutlet = null;
    }
}
```

Однако в iOS 6, он больше не нужно вызывать `ReleaseDesignerOutlets`.   
   
   
   
Для кода очистки, iOS 6 приложения должны использовать `DidReceiveMemoryWarning`. Однако код, который вызывает метод `Dispose` должно проводиться расчетливо и только для служб с интенсивными вычислениями объектов памяти как показано ниже:

```csharp
if (myImageView != null){
    if (myImageView.Superview == null){
        myImageView.Dispose();
        myImageView = null;
    }
}
```

Опять же, вызвав `Dispose` как описано выше редко требуется. В общем, большинство приложений должны сделать заключается в удалении обработчиков событий.

В случае сохранения состояния приложения могут выполнять в `ViewWillDisappear` и `ViewDidDisappear` вместо `ViewWillUnload`.


### <a name="iphone-5-resolution"></a>iPhone 5 разрешения

устройства iPhone 5 имеют разрешение 640 x 1136. Приложений, нацеленных на предыдущие версии iOS будет отображаться letterboxed при запуске на iPhone 5, как показано ниже:

 [![](images/01-letterboxed.png "Приложений, нацеленных на предыдущие версии iOS будет отображаться letterboxed при запуске на iPhone 5")](images/01-letterboxed.png#lightbox)

В приложение для отображения в полноэкранном режиме на iPhone 5, просто добавьте образ с именем `Default-568h@2x.png` с разрешением 640 x 1136. На следующем рисунке показан на приложение, после этого образ был включен:

 [![](images/02-fullscreen.png "На этом снимке экрана показано приложение, после этого образ был включен")](images/02-fullscreen.png#lightbox)

### <a name="subclassing-uinavigationbar"></a>Создание подклассов UINavigationBar

В iOS 6 `UINavigationBar` может быть подклассом. Это позволяет дополнительный контроль внешний вид `UINavigationBar`. Например, приложения могут подкласс для добавления представлений, которого должна начаться анимация эти представления и изменения границ `UINavigationBar`.

В следующем примере кода показан пример подклассов `UINavigationBar` , добавляющий `UIImageView`:

```csharp
public class CustomNavBar : UINavigationBar
{
    UIImageView iv;
    public CustomNavBar (IntPtr h) : base(h)
    {
        iv = new UIImageView (UIImage.FromFile ("monkey.png"));
        iv.Frame = new CGRect (75, 0, 30, 39);
    }
    public override void Draw (RectangleF rect)
    {
        base.Draw (rect);
        TintColor = UIColor.Purple;
        AddSubview (iv);
    }
}
```

Чтобы добавить подклассов `UINavigationBar` для `UINavigationController`, используйте `UINavigationController` конструктор, принимающий тип `UINavigationBar` и `UIToolbar`, как показано ниже:

```csharp
navController = new UINavigationController (typeof(CustomNavBar), typeof(UIToolbar));
```

С помощью этого `UINavigationBar` подкласс приводит представление изображения, отображаемого, как показано на следующем снимке экрана:

 [![](images/03-navbar.png "С помощью этого UINavigationBar подкласс результаты в представлении изображения, отображаемого, как показано на этом снимке экрана")](images/03-navbar.png#lightbox)

### <a name="interface-orientation"></a>Интерфейс ориентации

До iOS 6 приложения может переопределять `ShouldAutorotateToInterfaceOrientation`, возвращая true для любой ориентации поддерживается конкретном контроллере. Например следующий код будет использоваться для поддержки только Книжная:

```csharp
public override bool ShouldAutorotateToInterfaceOrientation (UIInterfaceOrientation toInterfaceOrientation)
    {
        return (toInterfaceOrientation == UIInterfaceOrientation.Portrait);
    }
```

В iOS 6 `ShouldAutorotateToInterfaceOrientation` является устаревшим.
Вместо этого приложения могут переопределить `GetSupportedInterfaceOrientations` на контроллере корневого представление, как показано ниже:

```csharp
public override UIInterfaceOrientationMask GetSupportedInterfaceOrientations ()
    {
        return UIInterfaceOrientationMask.Portrait;
    }
```

На iPad, по умолчанию все четыре ориентации Если `GetSupportedInterfaceOrientation` не реализован. На iPhone и iPod Touch, значение по умолчанию — все ориентации, за исключением `PortraitUpsideDown`.
