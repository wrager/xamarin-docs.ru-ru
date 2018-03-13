---
title: "Макет для приложений, планшетных и настольных компьютеров"
ms.topic: article
ms.prod: xamarin
ms.assetid: D62F472B-4345-4983-8403-659A538B591F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/01/2016
ms.openlocfilehash: f870cda73625197fb15bf19be1cdabbd675124d6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="layout-for-tablet-and-desktop-apps"></a>Макет для приложений, планшетных и настольных компьютеров

Xamarin.Forms поддерживает все типы устройств на поддерживаемых платформах, что помимо телефоны, приложения могут выполняться:

* iPad,
* Планшеты Android,
* Windows планшетов и настольных компьютеров (под управлением Windows 8.1 или Windows 10).

Здесь приводится краткое описание:

* поддерживаемые [типы устройств](#Device_Types), и
* как [оптимизировать](#optimize) макеты для планшетов и телефонов.

<a name="Device_Types" />

## <a name="device-types"></a>Типы устройств

Большего размера экрана устройства, доступны для всех платформ, поддерживаемых Xamarin.Forms.

### <a name="ipads-ios"></a>устройств iPad (iOS)

Шаблон Xamarin.Forms автоматически включает поддержку iPad, настроив **Info.plist > устройства** параметру **универсальной** (что означает iPhone и iPad поддерживаются).

Для запуска приятной работы и обеспечить разрешение полного экрана используется на всех устройствах, следует убедиться в том [экрана запуска iPad](~/ios/app-fundamentals/images-icons/launch-screens.md) предоставляется (с помощью раскадровки). Это гарантирует, что приложение правильно отображается на устройствах iPad mini, iPad и iPad Pro.

До iOS 9 заняла все приложения на полный экран на устройстве, но теперь можно выполнить некоторые устройств iPad [разбиение экрана многозадачной](~/ios/platform/multitasking.md).
Это означает, что приложение может занять просто тонкий столбец сбоку экрана, 50% от ширины экрана, или во весь экран.

[![](tablet-images/ipad-sml.png "iPad пример экрана разбиение")](tablet-images/ipad.png#lightbox "iPad пример экрана разбиение")

Функциональные возможности разделенного окна означает, что следует разрабатывать приложения для работы с от 320 пикселей в ширину или настолько, насколько 1366 пикселей в ширину.

### <a name="android-tablets"></a>Планшеты Android

Android экосистема имеет множество поддерживаемых размеров, от небольших телефонов до больших планшетных ПК. Xamarin.Forms может поддерживать все размерами экранов, но как с другими платформами, может потребоваться настроить пользовательский интерфейс для большего устройств.

При поддержке много различных разрешений экрана, можно предоставить ресурсы образов в машинном коде в различных размеров, чтобы оптимизировать взаимодействие с пользователем.
Просмотрите [Android ресурсов](~/android/app-fundamentals/resources-in-android/index.md) документации (и в частности [Создание ресурсов для изменения размеров](~/android/app-fundamentals/resources-in-android/resources-for-varying-screens.md)) Дополнительные сведения о том, как структура папок и имена файлов в приложении Android проект для включения оптимизированного образа ресурсы в приложение.

### <a name="windows-tablets-and-desktops"></a>Windows планшетов и настольных компьютеров

Для поддержки планшетов и настольных компьютеров под управлением Windows, необходимо использовать один из двух типов поддерживаемых проекта:

* [Windows 8.1](~/xamarin-forms/platform/windows/installation/tablet.md) -
  выполняет построение приложения для Windows 8.1 планшетов и настольных компьютеров.
* [Поддержка Windows UWP](~/xamarin-forms/platform/windows/installation/universal.md) -
  строит универсальных приложений, работающих на их как телефоны Windows 10, планшетных ПК и рабочим столам.

Приложения, работающие в Windows планшетов и настольных компьютеров может быть увеличена произвольный измерениями Кроме работает полноэкранном режиме.

[![](tablet-images/splitscreen-sml.png "Windows разбиение пример экрана")](tablet-images/splitscreen.png#lightbox "Windows разбиение пример экрана")


<a name="optimize" />

## <a name="optimizing-for-tablet-and-desktop"></a>Оптимизация для планшетных и настольных компьютеров

Можно настроить пользовательский интерфейс Xamarin.Forms в зависимости от того, ли телефон или планшет или рабочего стола устройство уже используется. Это означает, что вы можете оптимизировать работу пользователя для устройств большим экраном, таких как планшетные ПК и настольных компьютеров.


### <a name="deviceidiom"></a>Device.Idiom

Можно использовать [ `Device` ](~/xamarin-forms/platform/device.md) классе, чтобы изменить поведение вашего приложения или в пользовательском интерфейсе. С помощью `Device.Idiom` перечисления, вы можете

```csharp
if (Device.Idiom == TargetIdiom.Phone)
{
    HeroImage.Source = ImageSource.FromFile("hero.jpg");
} else {
    HeroImage.Source = ImageSource.FromFile("herotablet.jpg");
}
```

Этот подход можно развернуть для выполнения значительных изменений в макеты отдельных страниц, или даже для отрисовки совершенно различными страницами на экраны больше.

### <a name="leveraging-masterdetailpage"></a>Использование MasterDetailPage

[ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Идеально подходит для больших экранов, особенно на iPad, где он использует [ `UISplitViewController` ](https://developer.xamarin.com/api/type/UIKit.UISplitViewController/) для взаимодействия с машинным кодом iOS.

Просмотрите [этой записи блога Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/) чтобы увидеть, как адаптировать пользовательского интерфейса, чтобы телефоны использовать один макет и экраны больше можно использовать другое (с `MasterDetailPage`).



## <a name="related-links"></a>Связанные ссылки

- [Блог Xamarin](https://blog.xamarin.com/bringing-xamarin-forms-apps-to-tablets/)
- [Образец MyShoppe](https://github.com/jamesmontemagno/myshoppe)
