---
title: "Предоставление обратной совместимости с пакетом поддержки Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7511D2F8-2B4F-4200-C74E-E967153B2E8D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/12/2017
ms.openlocfilehash: f1567815ec342a958b48ec4801e2918f2981de3d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="providing-backwards-compatibility-with-the-android-support-package"></a>Предоставление обратной совместимости с пакетом поддержки Android

Полезность фрагменты будут ограничены без обратной совместимости с предварительно Android 3.0 устройств (API уровня 11). Для обеспечения этой возможности, появившиеся Google [библиотека поддержки](http://developer.android.com/sdk/compatibility-library.html) (исходное название *библиотеки Android совместимости* при выпуске) какие backports некоторых API из более новых версий Android в более старых версиях Android. Это Android, позволяющий устройства под управлением Android версии 1.6 (API уровня 4) для Android 2.3.3 пакет поддержки. (API уровня 10).

> [!NOTE]
> **Примечание**: только `ListFragment` и `DialogFragment` можно загрузить с помощью пакета Android поддержки. Ни один из другой фрагмент подклассов, такие как `PreferenceFragment,` поддерживаются в Android пакет поддержки. Они не будут работать в предварительно Android 3,0 приложений. 

<a name="Adding_the_Support_Package" /> 

## <a name="adding-the-support-package"></a>Добавление пакета поддержки

Пакет поддержки Android не добавляется автоматически в Xamarin.Android приложение. Xamarin предоставляет [пакет NuGet библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) для упрощения процедуры добавления библиотеки поддержки Xamarin.Android приложению. Для включения поддержки пакетов в вашей Xamarin.Android приложения включают [библиотеку поддержки Android версии 4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) компонента в проект Xamarin.Android, как показано на следующем снимке экрана: 

[![Снимок экрана для поддержки библиотеки Android версии 4 пакет добавляется в проект](providing-backwards-compatibility-images/02.png)](providing-backwards-compatibility-images/02.png)

После выполнения этих шагов становится можно использовать фрагменты в более ранних версиях Android. API-интерфейсы фрагмент будет работать сейчас же в более ранних версиях, за исключением следующих случаев: 

-   **Изменить Минимальная версия Android** &ndash; приложение больше не нужна для Android 3.0 или более поздней версии, как показано ниже: 

    [![Целевой объект экрана Android по минимальное значение в группе свойств приложения](providing-backwards-compatibility-images/03.png)](providing-backwards-compatibility-images/03.png)

-   **Расширить FragmentActivity** &ndash; действия, размещающими фрагментов теперь должен наследовать от `Android.Support.V4.App.FragmentActivity` , а не из `Android.App.Activity` . 

-   **Обновление пространства имен** &ndash; классы, наследующие от `Android.App.Fragment` теперь должен наследовать от `Android.Support.V4.App.Fragment` . Удаление с помощью инструкции « `using Android.App;` » в верхней части исходного кода и замените его с « `using Android.Support.V4.App` ». 

-   **Используйте SupportFragmentManager** &ndash; `Android.Support.V4.App.FragmentActivity` предоставляет `SupportingFragmentManager` свойство, которое необходимо использовать для получения ссылки на `FragmentManager` . Пример: 

```csharp
FragmentTransaction fragmentTx = this.SupportingFragmentManager.BeginTransaction();
DetailsFragment detailsFrag = new DetailsFragment();
fragmentTx.Add(Resource.Id.fragment_container, detailsFrag);
fragmentTx.Commit();
```

Эти изменения на месте она будет невозможно запустить приложение на основе фрагмент на Android 1.6 или 2.x, а также на Honeycomb и Южные Сандвичевы мороженого. 


## <a name="related-links"></a>Связанные ссылки

- [Поддержка Android библиотеки v4 NuGet](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
