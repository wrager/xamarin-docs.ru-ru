---
title: "Пошаговое руководство. Создание пользовательского интерфейса с вкладками с TabHost"
description: "В этой статье приведены шаги создания пользовательского интерфейса с вкладками в Xamarin.Android, с помощью TabHost API."
ms.topic: article
ms.prod: xamarin
ms.assetid: AD6E2173-974E-477C-940F-0CAB5E53326D
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 2dd397e824ce7735be4421c3f258852de3f77ecb
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="walkthrough---creating-a-tabbed-ui-with-tabhost"></a>Пошаговое руководство. Создание пользовательского интерфейса с вкладками с TabHost

_В этой статье приведены шаги создания пользовательского интерфейса с вкладками в Xamarin.Android, с помощью TabHost API._

> [!NOTE]
> `TabHost` — Это старый API, рекомендуется к использованию с Google. Разработчики могут создавать приложения с вкладками, с помощью [панели действий](~/android/user-interface/controls/action-bar.md). `ActionBar` Доступна в все версии Android. Он впервые появился в Android 3.0 (API уровня 11) и обратно была перенесена в ОС Android 2.2 (API уровня 8) и Android 2.3 (API уровня 10) в [V7 AppCompat библиотеки](http://developer.android.com/tools/support-library/features.html#v7-appcompat), которая доступна для Xamarin.Android через [Xamarin Библиотека поддержки Android - V7](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/) пакета.

В этой статье рассмотрена Создание пользовательского интерфейса с вкладками в Xamarin.Android с помощью `TabHost` API. Это старые API, которая доступна во всех версиях Android. В этом примере создается приложение с тремя вкладками с логикой для каждой вкладки, инкапсулируются в действии.
Снимке экрана ниже приведен пример приложения, который будет создан.

![Снимок экрана примера приложения с несколькими вкладками](creating-a-tabbed-ui-images/image02.png)


## <a name="creating-the-application"></a>Создание приложения

Загрузите и распакуйте [TabHostWalkthrough](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/).
Этот проект служит отправной точкой для нашего приложения и содержит несколько образов. Если изучить этот проект, вы увидите, что мы уже создали drawable ресурсы для вкладки значков.

Первый позволяет обновить файл макета **Resources/Layout/Main.axml** , где будет размещаться вкладок. Изменить **Resources/Layout/Main.axml** и вставьте следующий XML-код:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TabHost xmlns:android="http://schemas.android.com/apk/res/android"
         android:id="@android:id/tabhost"
         android:layout_width="fill_parent"
         android:layout_height="fill_parent">
    <LinearLayout
            android:orientation="vertical"
            android:layout_width="fill_parent"
            android:layout_height="fill_parent"
            android:padding="5dp">
        <TabWidget
                android:id="@android:id/tabs"
                android:layout_width="fill_parent"
                android:layout_height="wrap_content" />
        <FrameLayout
                android:id="@android:id/tabcontent"
                android:layout_width="fill_parent"
                android:layout_height="fill_parent"
                android:padding="5dp" />
    </LinearLayout>
</TabHost>
```

На следующем рисунке показан макет в конструкторе Xamarin:

[![Снимок экрана: макет TabHost в конструкторе Xamarin](creating-a-tabbed-ui-images/image04-sml.png)](creating-a-tabbed-ui-images/image04.png#lightbox)

TabHost должен иметь два дочерних представлений внутри него: `TabWidget` и `FrameLayout`. Положение `TabWidget` и `FrameLayout` по вертикали внутри `TabHost`, `LinearLayout` используется. FrameLayout — где для каждой вкладки наполнения, который является пустым поскольку `TabHost` будет автоматически внедряют каждого действия во время выполнения. Существует несколько правил, которые необходимо учитывать, когда дело доходит до создания макета с вкладками пользовательских интерфейсов.

-   `TabHost` Должен иметь идентификатор `@android:id/tabhost`.

-   `TabWidget` Должен иметь идентификатор `@android:id/tabs`.

-   `FrameLayout` Должен иметь идентификатор `@android:id/tabcontent`.

-   `TabHost` требуется любого действия, который им управляет наследование от `TabActivity`. Таким образом, важно подкласс `TabActivity` здесь &ndash; типичных действий не будет работать.

Измените файл **MainActivity.cs** , чтобы класс `MainActivity` подклассов `TabActivity` как показано в следующем фрагменте кода:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/ic_launcher")]
public class MainActivity : TabActivity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.Main);
    }
}
```

Создайте четыре отдельные классы действий в проекте: `MyScheduleActivity`, `SessionsActivity`, `SpeakersActivity`, и `WhatsOnActivity`. Каждое действие будет создания пользовательского интерфейса вкладки. Теперь эти действия рассматриваются в заглушку, отображает `TextView` с простое сообщение. Редактировать код в каждое действие, чтобы содержать следующие `OnCreate` реализации:

```csharp
[Activity]
public class MyScheduleActivity : Activity
{
    protected override void OnCreate (Bundle savedInstanceState)
    {
        base.OnCreate (savedInstanceState);
        TextView textview = new TextView (this);
        textview.Text = "This is the My Schedule tab";
        SetContentView (textview);
    }
}
```

Обратите внимание, что приведенный выше код не использует файл макета. Он создает `TextView` текст и устанавливает, `TextView` как представление содержимого. Повторяющийся это для каждой из оставшихся трех действий.

Далее мы назначение значков для каждой из вкладок. Каждая вкладка требуется два значка &ndash; для выполнения при выбранном состоянии и невыбранном состоянии. Примером эти два разных значка можно увидеть в следующие два образа (значки, необходимые для этого приложения уже были добавлены в образец проекта):

![Снимок экрана состояния и значки невыбранном состоянии](creating-a-tabbed-ui-images/tab-icons.png)


Мы назначит drawable ресурсы значок вкладки, определив *спискасостоянийDrawable*. Состояние списка drawables — это специальные drawable ресурсы, которые определены в формате XML и позволяют указать различные изображения, характерные для этого элемента в состояние. В этом примере имеется одно изображение, используемое при выборе вкладки, а другой используется, если не установлен. Чтобы сэкономить время, необходимые drawables списка состояний были добавлены в проект. В следующем списке приведены файлы и содержит XML.

-   `ic_tab_my_schedule.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

-   `ic_tab_sessions.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_sessions_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_sessions_unselected"/>
    </selector>
    ```

-   `ic_tab_speakers.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_speakers_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_speakers_unselected"/>
    </selector>
    ```

-   `ic_tab_whats_on.xml`

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <selector xmlns:android="http://schemas.android.com/apk/res/android">
        <item android:drawable="@drawable/ic_tab_whats_on_selected"
              android:state_selected="true"/>
        <item android:drawable="@drawable/ic_tab_whats_on_unselected"/>
    </selector>
    ```

Вкладки добавляются `TabHost` программным способом, который является очень повторяющиеся задачи. Чтобы упростить это, добавьте следующий метод в класс `MainActivity`:

```csharp
private void CreateTab(Type activityType, string tag, string label, int drawableId )
{
    var intent = new Intent(this, activityType);
    intent.AddFlags(ActivityFlags.NewTask);

    var spec = TabHost.NewTabSpec(tag);
    var drawableIcon = Resources.GetDrawable(drawableId);
    spec.SetIndicator(label, drawableIcon);
    spec.SetContent(intent);

    TabHost.AddTab(spec);
}
```

Каждая вкладка в `TabHost` представлен экземпляр из `TabHost.TabSpec` класса. Этот экземпляр содержит метаданные-необходимые для подготовки к просмотру на вкладку, в частности:

-   **Текст и значок** &ndash; для отображения в `TabWidget`.

-   **Вкладка содержимого** &ndash; это может быть либо `Activity` или `View` и отображается при выборе вкладки.

-   **Уникальный тег** &ndash; каждой вкладки должно быть уникальным тега, назначенного к нему.

Необходимо добавить `TabHost.TabSpec` экземпляра для каждой вкладки в приложении. Позволяет сделать это на следующем шаге.

Обновить метод `OnCreate` в `MainActivity` , как показано в следующем коде:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateTab(typeof(WhatsOnActivity), "whats_on", "What's On", Resource.Drawable.ic_tab_whats_on);
    CreateTab(typeof(SpeakersActivity), "speakers", "Speakers", Resource.Drawable.ic_tab_speakers);
    CreateTab(typeof(SessionsActivity), "sessions", "Sessions", Resource.Drawable.ic_tab_sessions);
    CreateTab(typeof(MyScheduleActivity), "my_schedule", "My Schedule", Resource.Drawable.ic_tab_my_schedule);
}
```

Запустите приложение. Приложения должны выглядеть как снимок экрана, показанный в начале этого пошагового руководства.

Вот и все! Мы создали с вкладками приложения, который позволяет легко перемещаться к различным частям приложения.



## <a name="summary"></a>Сводка

В этой главе описано с вкладками макеты и предложены процесс создания приложения с вкладками. Пошаговое руководство было показано, как использовать `TabActivity` увеличению макет файла, размещение `TabHost` и `TabWidget`. `TabHost` Был затем заполняется коллекцию `TabHost.TabSpec` объектов, которая будет использоваться службой `TabHost` во время выполнения для создания экземпляра действия, которые будут использоваться на каждой из вкладок.



## <a name="related-links"></a>Связанные ссылки

- [TabHostWalkthrough (пример)](https://developer.xamarin.com/samples/monodroid/UserInterface/TabHostWalkthrough/)
- [TabHost](https://developer.xamarin.com/api/type/Android.Widget.TabHost/)
- [TabWidget](https://developer.xamarin.com/api/type/Android.Widget.TabWidget/)
- [TabActivity](https://developer.xamarin.com/api/type/Android.App.TabActivity/)
- [ActionBar](http://developer.android.com/guide/topics/ui/actionbar.html)
- [Пакет NuGet AppCompat v7 библиотеку поддержки Android](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)
- [Библиотека v7 совместимости приложений](http://developer.android.com/tools/support-library/features.html#v7-appcompat)
