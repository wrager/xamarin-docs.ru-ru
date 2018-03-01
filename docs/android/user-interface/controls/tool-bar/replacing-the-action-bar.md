---
title: "Замена панели действий"
ms.topic: article
ms.prod: xamarin
ms.assetid: 5341D28E-B203-478D-8464-6FAFDC3A4110
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 91d5612991c2297418cf7003c499c1a1bbfc7558
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="replacing-the-action-bar"></a>Замена панели действий

<a name="overview" />

## <a name="overview"></a>Обзор

Одно из наиболее распространенных применений `Toolbar` является замена пользовательской панели действий по умолчанию `Toolbar` (при создании нового проекта Android, он использует панели действий по умолчанию). Поскольку `Toolbar` предоставляет возможность добавлять фирменные эмблемы, заголовки, пункты меню, кнопки навигации и даже настраиваемые представления в панели приложения части действия пользовательского интерфейса, он обеспечивает значительные обновления на панели действий по умолчанию.

Для замены приложения по умолчанию действие панель с `Toolbar`: 

1.  Создать новые пользовательские темы и изменить свойства приложения, чтобы он использовал эту новую тему. 

2.  Отключить `windowActionBar` в пользовательские темы, а включить `windowNoTitle` атрибута.

3.  Задать макет для `Toolbar`.

4.  Включить `Toolbar` макета в действии **Main.axml** файл макета. 

5.  Добавьте код к действию `OnCreate` метод нахождения `Toolbar` и вызвать `SetActionBar` установка `ToolBar` как панели действий.

Этот процесс подробно в следующих разделах. Создается простое приложение, и заменить ее действие строки с настраиваемый `Toolbar`. 


<a name="start_project" />

## <a name="start-an-app-project"></a>Запуск проекта приложения

Создайте новый проект Android с именем **ToolbarFun** (см. [Привет, Android](~/android/get-started/hello-android/hello-android-quickstart.md) Дополнительные сведения о создании нового проекта Android). После создания этого проекта значение целевой и менее уровни Android API **Android 5.0 (API уровня 21 - без описания операций)**. Дополнительные сведения о задании версии Android уровней см. в разделе [основные сведения об уровнях Android API](~/android/app-fundamentals/android-api-levels.md). При построении и запуска приложения отображает панели действий по умолчанию, как показано на этом снимке экрана: 

[![Снимок экрана: панель действий по умолчанию](replacing-the-action-bar-images/01-before-sml.png)](replacing-the-action-bar-images/01-before.png)


<a name="custom_theme" />

## <a name="create-a-custom-theme"></a>Создайте пользовательскую тему

Откройте **ресурсы или значений** каталог и создайте новый файл с именем **styles.xml**. Замените его содержимое следующим кодом XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<resources>
  <style name="MyTheme" parent="@android:style/Theme.Material.Light.DarkActionBar">
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowActionBar">false</item>
    <item name="android:colorPrimary">#5A8622</item>
  </style>
</resources>
```

Этот XML-документ определяет новый пользовательский тема с именем **MyTheme** , основанный на **Theme.Material.Light.DarkActionBar** темы в без описания операций. `windowNoTitle` Атрибута задано значение `true` Чтобы скрыть строку заголовка: 

```xml
<item name="android:windowNoTitle">true</item>
```

Чтобы отобразить пользовательскую панель инструментов по умолчанию `ActionBar` необходимо отключить: 

```xml
<item name="android:windowActionBar">false</item>
```

Olive-green `colorPrimary` параметр используется для цвета фона панели инструментов: 
 
```xml
<item name="android:colorPrimary">#5A8622</item>
```

Изменить **Properties/AndroidManifest.xml** и добавьте следующее `android:theme` атрибут `<application>` элемент, чтобы приложение использовало `MyTheme` пользовательские темы: 

```xml
<application android:label="@string/app_name" android:theme="@style/MyTheme"></application>
```

Дополнительные сведения о применении пользовательские темы для приложения см. в разделе [пользовательские темы, с помощью](~/android/user-interface/material-theme.md#customtheme). 


<a name="toolbar_layout" />

## <a name="define-a-toolbar-layout"></a>Определить расположение панели инструментов

В **ресурсы и макет** каталога, создайте новый файл с именем **toolbar.xml**. Замените его содержимое следующим кодом XML: 

```xml
<?xml version="1.0" encoding="utf-8"?>
<Toolbar xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/toolbar"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:minHeight="?android:attr/actionBarSize"
    android:background="?android:attr/colorPrimary"
    android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"/>
```

Этот XML-код определяет пользовательский `Toolbar` , заменяющая панели действий по умолчанию. Минимальная высота `Toolbar` установлен размер панели действий, он заменяет: 

```csharp
android:minHeight="?android:attr/actionBarSize"
```

Цвет фона `Toolbar` равно olive-green цветом, определенным ранее в **styles.xml**:

```csharp
android:background="?android:attr/colorPrimary"
```

Начиная с без описания операций, `android:theme` атрибут может использоваться для определения стиля отдельного представления. `ThemeOverlay.Material` Темы, представленные в без описания операций делают возможным наложения значение по умолчанию `Theme.Material` темы, соответствующие атрибуты, чтобы сделать их Темная или Светлая перезапись. В этом примере `Toolbar` использует темной темой, чтобы его содержимое является светлым: 

```csharp
android:theme="@android:style/ThemeOverlay.Material.Dark.ActionBar"
```

Этот параметр используется, чтобы элементы меню противоположен темный цвет фона.


<a name="include_layout" />

## <a name="include-the-toolbar-layout"></a>Включить макет панели инструментов

Измените файл макета **Resources/layout/Main.axml** и замените его содержимое следующим кодом XML:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <include
        android:id="@+id/toolbar"
        layout="@layout/toolbar" />
    <Button
        android:id="@+id/MyButton"
        android:layout_below="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Hello World, Click Me!" />
</RelativeLayout>
```

Этот макет включает `Toolbar` определенные в **toolbar.xml** и использует `RelativeLayout` позволяет указать, что `Toolbar` следует поместить в верхнюю часть пользовательского интерфейса (над кнопкой). 


<a name="activate_toolbar" />

## <a name="find-and-activate-the-toolbar"></a>Поиск и активировать панель инструментов

Изменить **MainActivity.cs** и добавьте следующий код с помощью инструкции:

```csharp
using Android.Views;
```

Кроме того, добавьте следующий код в конец `OnCreate` метод:

```csharp
var toolbar = FindViewById<Toolbar>(Resource.Id.toolbar);
SetActionBar(toolbar);
ActionBar.Title = "My Toolbar";
```

Этот код находит `Toolbar` и вызывает метод `SetActionBar` , чтобы `Toolbar` будут присутствовать характеристики панели действий по умолчанию. Заголовок панели меняется на **Мои инструментов**. Как показано в этом примере кода `ToolBar` напрямую ссылаться как на панели действий. Скомпилируйте и запустите это приложение &ndash; пользовательское `Toolbar` отображения вместо панели действий по умолчанию: 

[![Снимок экрана пользовательскую панель инструментов с зеленым цветом схемы](replacing-the-action-bar-images/02-after-sml.png)](replacing-the-action-bar-images/02-after.png)

Обратите внимание, что `Toolbar` построен таким образом, независимо от `Theme.Material.Light.DarkActionBar` темы, применяемой к оставшейся части приложения. 


<a name="main_menus" />
 
## <a name="add-menu-items"></a>Добавление элементов меню 

В этом разделе добавляются в меню `Toolbar`. Верхней правой области `ToolBar` зарезервирован для пунктов меню &ndash; каждым пунктом меню (также называемый *элемента действия*) можно выполнять действия в рамках текущего действия или он может выполнять действия от имени всего приложения. 

Чтобы добавить меню к `Toolbar`: 

1.  Добавление значков (при необходимости) `mipmap-` папки проекта приложения. Компания Google предоставляет набор значков свободного меню на [материала значки](https://design.google.com/icons/) страницы. 

2.  Определить содержимое этих пунктов меню путем добавления нового файла ресурсов меню в разделе **ресурсы или меню**. 

3.  Реализуйте `OnCreateOptionsMenu` метод действия &ndash; этот метод увеличивает пунктов меню. 

4.  Реализуйте `OnOptionsItemSelected` метод действия &ndash; этот метод выполняет действие при касании элемента меню. 

В следующих разделах приводится подробное описание этого процесса путем добавления **изменить** и **Сохранить** элементы меню, пользовательское `Toolbar`. 


<a name="menu_icons" />

### <a name="install-menu-icons"></a>Установка значков

Продолжить `ToolbarFun` примера приложения, добавьте в проект приложения значков. Загрузить [icons.zip инструментов](https://github.com/xamarin/monodroid-samples/blob/master/Supportv7/AppCompat/Toolbar/Resources/toolbar-icons.zip?raw=true) и распакуйте его. Скопируйте содержимое извлеченный *MIP-карты -* папки в проект *MIP-карты -* папки в **ToolbarFun/ресурсы** и включить каждого файла значка добавлены в проект.

<a name="menu_resource" />

### <a name="define-a-menu-resource"></a>Определить ресурс меню

Создайте новый **меню** подкаталогу **ресурсов**. В **меню** подкаталог, создайте новый файл ресурсов меню с именем **top_menus.xml** и замените его содержимое следующим кодом XML: 

```xml
<?xml version="1.0" encoding="utf-8" ?>
<menu xmlns:android="http://schemas.android.com/apk/res/android">
  <item
       android:id="@+id/menu_edit"
       android:icon="@mipmap/ic_action_content_create"
       android:showAsAction="ifRoom"
       android:title="Edit" />
  <item
       android:id="@+id/menu_save"
       android:icon="@mipmap/ic_action_content_save"
       android:showAsAction="ifRoom"
       android:title="Save" />
  <item
       android:id="@+id/menu_preferences"
       android:showAsAction="never"
       android:title="Preferences" />
</menu>
```

Этот XML-код создает три элемента меню.

-   **Изменить** пункт меню, который использует `ic_action_content_create.png` значок (карандаша). 

-   Объект **Сохранить** пункт меню, который использует `ic_action_content_save.png` значок (дискету). 

-   Объект **предпочтения** пункт меню, у которого нет значка.

`showAsAction` Атрибуты **изменить** и **Сохранить** пунктов меню, присваивается `ifRoom` &ndash; этот параметр вызывает эти пункты меню отображаются в `Toolbar` при наличии Недостаточно места для отображения. **Предпочтения** наборов элементов меню `showAsAction` для `never` &ndash; в результате **предпочтения** меню будет отображаться в *переполнения* меню (три точки по вертикали). 

<a name="on_create_options_menu" />

### <a name="implement-oncreateoptionsmenu"></a>Реализуйте OnCreateOptionsMenu

Добавьте следующий метод **MainActivity.cs**:

```csharp
public override bool OnCreateOptionsMenu(IMenu menu)
{
    MenuInflater.Inflate(Resource.Menu.top_menus, menu);
    return base.OnCreateOptionsMenu(menu);
}
```

Android вызовы `OnCreateOptionsMenu` метод, чтобы приложение может указать ресурс меню для действия. В этом методе **top_menus.xml** завышенными ресурсов в передаваемый `menu`. Этот код вызывает новый **изменить**, **Сохранить**, и **предпочтения** пунктов меню в `Toolbar`. 


<a name="on_options_item_selected" />

### <a name="implement-onoptionsitemselected"></a>Реализуйте OnOptionsItemSelected

Добавьте следующий метод **MainActivity.cs**:

```csharp
public override bool OnOptionsItemSelected(IMenuItem item)
{
    Toast.MakeText(this, "Action selected: " + item.TitleFormatted,
        ToastLength.Short).Show();
    return base.OnOptionsItemSelected(item);
}
```

Когда пользователь касается пункта меню, вызывает Android `OnOptionsItemSelected` метод и передает в пункте меню, которая была выбрана. В этом примере реализация просто отображает всплывающее уведомление, чтобы указать, какой пункт меню был касании. 

Построение и запуск `ToolbarFun` новые элементы меню на панели инструментов. `Toolbar` Теперь отображает три значка меню, как показано на этом снимке экрана: 

[![Диаграмма иллюстрирующая расположения редактирования, сохранения и элементов меню переполнения](replacing-the-action-bar-images/04-menu-items-sml.png)](replacing-the-action-bar-images/04-menu-items.png)

После касания пользователь **изменить** отображения пункта меню, всплывающее уведомление указывает, что `OnOptionsItemSelected` был вызван метод: 

[![Снимок экрана из всплывающих отображается при касании изменение элементов](replacing-the-action-bar-images/05-toast-displayed-sml.png)](replacing-the-action-bar-images/05-toast-displayed.png)

Когда пользователь нажимает в меню переполнения **предпочтения** отображения пункта меню. Как правило, менее типичных действий должны размещаться в меню переполнения &ndash; в этом примере используется меню переполнения для **предпочтения** , так как он используется не так часто, как **изменить** и  **Сохранить**: 

[![Снимок экрана настройки пункт меню, который отображается в меню переполнения](replacing-the-action-bar-images/06-preferences-sml.png)](replacing-the-action-bar-images/06-preferences.png)

Дополнительные сведения о Android меню см. в разделе разработчика Android [меню](https://developer.android.com/guide/topics/ui/menus.html) раздела. 
 



## <a name="related-links"></a>Связанные ссылки

- [Панель инструментов без описания операций (пример)](https://developer.xamarin.com/samples/monodroid/android5.0/Toolbar/)
- [Панель инструментов AppCompat (пример)](https://developer.xamarin.com/samples/monodroid/Supportv7/AppCompat/Toolbar/)
