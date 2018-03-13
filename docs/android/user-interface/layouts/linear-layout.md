---
title: LinearLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: fc6bc9e1d4625f8f45887b0a144a31383046b296
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) — [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , отображающий дочерние [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) элементов в линейной направление, горизонтально или вертикально.

Следует уделить особое внимание чрезмерно используя [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
Если вы начинаете вложенности несколько [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s, вы можете рассмотреть возможность использования [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) вместо него.

Создание нового проекта с именем **HelloLinearLayout**.

Откройте **Resources/Layout/Main.axml** и вставьте следующий текст:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "fill_parent"
      android:layout_height=    "fill_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "fill_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "fill_parent"
    android:layout_height=    "fill_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "fill_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Тщательно изучите этот XML-документ. Корневой [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) , определяющий его ориентации на вертикальной &ndash; всех дочерних [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)s (для которого он имеет два) будут располагаться друг над другом вертикально. Первый дочерний элемент является другой [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) , использует горизонтальную ориентацию, а второй дочерний элемент является [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) , использующий вертикальной ориентацией. Каждый из этих вложенных [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s содержит несколько [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) элементы, которые ориентируются друг с другом таким образом определяется родительских [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Теперь откройте **HelloLinearLayout.cs** и убедитесь, что он загружает **Resources/Layout/Main.axml** макет в [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) метод:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

[ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Метод загружает файл макета для [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), указанной по Идентификатору ресурса &ndash; `Resources.Layout.Main` ссылается на **ресурсы и макета или Main.AXML** файл макета.

Запустите приложение. Вы увидите следующее:

[![Снимок экрана приложения первого LinearLayout располагаются горизонтально, второй по вертикали](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Обратите внимание на то, как XML-атрибуты определяют поведение каждого представления. Поэкспериментировать с различными значениями для `android:layout_weight` чтобы увидеть распределение площадь экрана на основе веса каждого элемента. В разделе [общих объектов макета](http://developer.android.com/guide/topics/ui/declaring-layout.html) документа для получения дополнительных сведений о том, как [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) дескрипторы `android:layout_weight` атрибута.


## <a name="references"></a>Ссылки

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Некоторые части этой страницы, изменения на основе работы создан и совместно используются Android открыть исходный проект и используются в соответствии с условиями, описанной в*
[*Creative Commons 2.5 однозначного соответствия лицензий* ](http://creativecommons.org/licenses/by/2.5/).

