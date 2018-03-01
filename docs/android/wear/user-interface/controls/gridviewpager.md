---
title: GridViewPager
ms.topic: article
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/02/2018
ms.openlocfilehash: f156d802d807c4087331ca5796b046f8f5f2fa0d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) образце показано, как реализовать шаблон навигации 2D выбора для Android с.

![Снимок экрана примера из GridViewPager на экране квадрат](gridviewpager-images/gridviewpager.png)

Сначала добавьте [Xamarin Android поддержки носят](http://www.nuget.org/packages/Xamarin.Android.Wear/) пакет NuGet для проекта.

Макет XML выглядит следующим образом:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Создание [ `GridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html) (или подкласс, такие как [ `FragmentGridPagerAdapter` ](http://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html) для предоставления представлений для отображения при переходе.

[Адаптера образец](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) показано, как реализовать требуемые методы, включая переопределения для `RowCount`, `GetColumnCount`, `GetBackground`, и `GetFragment`

Подключите адаптер, как показано:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Связанные ссылки

- [2D выбора doc Google](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Android.support.wearable документы](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (пример)](https://developer.xamarin.com/samples/GridViewPager/)
