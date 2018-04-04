---
title: Добавление AppCompat и материалов разработки
description: Выполните следующие действия для преобразования существующих приложений Xamarin.Forms Android использование совместимости приложений и разработки материалы
ms.prod: xamarin
ms.assetid: 045FBCDF-4D45-48BB-9911-BD3938C87D58
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/27/2017
ms.openlocfilehash: 8f9820b863274453cff7e4124df683fb8518a978
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="adding-appcompat-and-material-design"></a>Добавление AppCompat и материалов разработки

_Выполните следующие действия для преобразования существующих приложений Xamarin.Forms Android использование совместимости приложений и разработки материалы_

<!-- source https://gist.github.com/jassmith/a3b2a543f99126782936
https://blog.xamarin.com/material-design-for-your-xamarin-forms-android-apps/ -->

## <a name="overview"></a>Обзор

Эти инструкции описывают обновить существующие приложения Xamarin.Forms Android AppCompat библиотеки и включить материал конструктора в Xamarin.Forms приложений Android версии.

### <a name="1-update-xamarinforms"></a>1. Обновление Xamarin.Forms

Убедитесь, что решение использует Xamarin.Forms 2.0 или более поздней версии. При необходимости, обновите пакет Xamarin.Forms Nuget 2.0.

### <a name="2-check-android-version"></a>2. Проверка версии Android

Убедитесь, что проект Android целевая платформа — Android 6.0 (Marshmallow). Проверьте **проекта Android > Параметры > сборки > Общие** выбранные параметры, чтобы обеспечить corrent framework:

 ![](appcompat-images/target-android-6-sml.png "Конфигурации построения общие Android")

### <a name="3-add-new-themes-to-support-material-design"></a>3. Добавление новой темы для поддержки разработки материалы

Создайте следующие три файла в проекте Android и вставьте в него содержимое ниже. Компания Google предоставляет [по стилю](http://www.google.com/design/spec/style/color.html#color-color-palette) и [цветовой палитры генератор](http://www.materialpalette.com/) чтобы выбрать альтернативный цветовую схему, в соответствии с указанной.

**Resources/values/colors.xml**

```xml
<resources>
  <color name="primary">#2196F3</color>
  <color name="primaryDark">#1976D2</color>
  <color name="accent">#FFC107</color>
  <color name="window_background">#F5F5F5</color>
</resources>
```

**Resources/values/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
  </style>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light.NoActionBar">
    <item name="colorPrimary">@color/primary</item>
    <item name="colorPrimaryDark">@color/primaryDark</item>
    <item name="colorAccent">@color/accent</item>
    <item name="android:windowBackground">@color/window_background</item>
    <item name="windowActionModeOverlay">true</item>
  </style>
</resources>
```

Дополнительного стиля должен быть включен в **v21 значения** папки для применения определенных свойств при выполнении Android без описания операций и более поздних версиях.

**Resources/values-v21/style.xml**

```xml
<resources>
  <style name="MyTheme" parent="MyTheme.Base">
    <!--If you are using MasterDetailPage you will want to set these, else you can leave them out-->
    <!--<item name="android:windowDrawsSystemBarBackgrounds">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>-->
  </style>
</resources>
```

### <a name="4-update-androidmanifestxml"></a>4. Обновление AndroidManifest.xml

Чтобы обеспечить эту новую тему сведения — используется, и задайте тему в **AndroidManifest** файла путем добавления `android:theme="@style/MyTheme"` (оставить остальная часть XML, в котором он был).

**Properties/AndroidManifest.xml**

```xml
...
<application android:label="AppName" android:icon="@drawable/icon"
  android:theme="@style/MyTheme">
...
```

### <a name="5-provide-toolbar-and-tab-layouts"></a>5. Укажите макеты панели инструментов и вкладки

Создание **Tabbar.axml** и **Toolbar.axml** файлы в **ресурсы и макет** каталоге и вставьте в их содержимое из ниже:

**Resources/layout/Tabbar.axml**

```xml
<android.support.design.widget.TabLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/sliding_tabs"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:tabIndicatorColor="@android:color/white"
    app:tabGravity="fill"
    app:tabMode="fixed" />
```

Заданы свойства несколько вкладок, включая тяжести вкладки для `fill` и режим `fixed`.
Если у вас много вкладок можно отключить эту для прокрутки - прочтите Android [TabLayout документации](http://developer.android.com/reference/android/support/design/widget/TabLayout.html) для получения дополнительных сведений.

**Resources/layout/Toolbar.axml**

```xml
<android.support.v7.widget.Toolbar
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="?attr/actionBarSize"
    android:minHeight="?attr/actionBarSize"
    android:background="?attr/colorPrimary"
    android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
    app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
    app:layout_scrollFlags="scroll|enterAlways" />
```

В этих файлах мы создаем конкретной темы для панели инструментов, которые могут быть различными для вашего приложения.
Ссылаться на [Hello инструментов](https://blog.xamarin.com/android-tips-hello-toolbar-goodbye-action-bar/) запись блога, чтобы получить дополнительные сведения.


### <a name="6-update-the-mainactivity"></a>6. Обновление `MainActivity`

В существующих приложениях Xamarin.Forms **MainActivity.cs** класс будет наследовать от `FormsApplicationActivity`. Оно должно быть заменено `FormsAppCompatActivity` для включения новых функций.

**MainActivity.cs**

```csharp
public class MainActivity : FormsAppCompatActivity  // was FormsApplicationActivity
```

Наконец, «подключить» новые макеты на шаге 5 в `OnCreate` метода, как показано ниже:

```csharp
protected override void OnCreate(Bundle bundle)
{
  // set the layout resources first
  FormsAppCompatActivity.ToolbarResource = Resource.Layout.Toolbar;
  FormsAppCompatActivity.TabLayoutResource = Resource.Layout.Tabbar;

  // then call base.OnCreate and the Xamarin.Forms methods
  base.OnCreate(bundle);
  Forms.Init(this, bundle);
  LoadApplication(new App());
}
```
