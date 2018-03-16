---
title: "Поддержка предварительной Honeycomb Android с помощью поддержки пакетов"
ms.topic: article
ms.prod: xamarin
ms.assetid: DACD0C14-5DDF-7BDE-6844-80550D301307
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/15/2018
ms.openlocfilehash: 109c1e0f16d3a288160b64ec6ff833e5b31c4efd
ms.sourcegitcommit: 028936cd2fe547963c1cf82343c3ee16f658089a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/16/2018
---
# <a name="supporting-pre-honeycomb-android-using-support-packages"></a>Поддержка предварительной Honeycomb Android с помощью поддержки пакетов

*Android пакет поддержки* состоит из библиотек, которые обратно порта некоторых новых API &ndash; например фрагментов &ndash; в более старых версиях Android. Таким образом, добавив Android пакет поддержки, можно запустим нашего приложения на устройствах Android 2.3, как показано в следующих экранах:

[![Фрагменты Пошаговое руководство и сведения об активности снимки экрана](supporting-pre-honeycomb-images/01-sml.png)](supporting-pre-honeycomb-images/01.png#lightbox)

## <a name="adding-the-support-package"></a>Добавление пакета поддержки

Пакет поддержки Android не добавляется автоматически в Xamarin.Android приложение. Xamarin предоставляет [пакет NuGet библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) для упрощения процедуры добавления библиотеки поддержки Xamarin.Android приложению.
Для включения поддержки пакетов в вашей Xamarin.Android приложения включают [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) компонента в проект Xamarin.Android, как показано на следующем снимке экрана:

[![Добавление пакета библиотеку поддержки Android версии 4](supporting-pre-honeycomb-images/02-sml.png)](supporting-pre-honeycomb-images/02.png#lightbox)

После добавления пакета измените целевую платформу для ОС Android 2.2 или более поздней версии:

[![Снимок экрана: изменение уровня API целевой платформы](supporting-pre-honeycomb-images/03-sml.png)](supporting-pre-honeycomb-images/03.png#lightbox)

Кроме того убедитесь, что минимальная версия Android обращается тот же уровень API:

[![Снимок экрана: задание Android Минимальная версия](supporting-pre-honeycomb-images/04-sml.png)](supporting-pre-honeycomb-images/04.png#lightbox)

### <a name="change-mainactivity-to-derive-from-fragmentactivity"></a>Изменение MainActivity для наследования от FragmentActivity

Любое действие, которое использует фрагментов, должен наследовать от `Xamarin.Support.V4.App.FragmentActivity`. Этот класс является неотъемлемой частью пакета поддержки, так как она допускает фрагментов размещаемой действиями, независимо от версии Android. `MainActivity` требуется только одно небольшое изменение — он теперь должен наследовать из `Android.Support.V4.App.FragmentActivity`:

```csharp
[Activity(Label = "Fragments Walkthrough", MainLauncher = true, Icon = "@drawable/launcher")]
public class MainActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       SetContentView(Resource.Layout.activity_main);
   }
}
```


### <a name="change-detailsactivity-to-derive-from-fragmentactivity"></a>Изменение DetailsActivity для наследования от FragmentActivity

`DetailsActivity` также необходимо изменить, из `Activity` для `FragmentActivity`. Как `FragmentManager` — не совместима с Honeycomb предварительной версии Android Android пакет поддержки содержит класс-оболочку, `SupportFragmentManager`, который обеспечивает обратную совместимость. Каждый `FragmentActivity` имеет `SupportFragmentManager` свойство, и `DetailsActivity` изменяется для использования `SupportFragmentManager` вместо `FragmentManager`:

```csharp
[Activity(Label = "Details Activity")]
public class DetailsActivity : Android.Support.V4.App.FragmentActivity
{
   protected override void OnCreate(Bundle bundle)
   {
       base.OnCreate(bundle);
       var index = Intent.Extras.GetInt("index", 0);
       var details = DetailsFragment.NewInstance(index);
       var fragmentTransaction = SupportFragmentManager.BeginTransaction(); // Notice the change from FragmentManager to SupportFragmentManager
       fragmentTransaction.Add(Android.Resource.Id.Content, details);
       fragmentTransaction.Commit();
   }
}
```

После завершения этих изменений мы теперь имеется приложение, можно запустить на Android версии 1.6 и более поздней версии и использующего фрагментов для настройки нашей пользовательского размера нашей целевое устройство.


## <a name="related-links"></a>Связанные ссылки

- [Android поддерживает библиотеку v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4)
