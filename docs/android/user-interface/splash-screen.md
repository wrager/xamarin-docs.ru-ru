---
title: Заставка
description: Приложение Android занимает некоторое время для запуска, особенно при первом запуске приложения на устройстве. Экрана-заставки может отображать начала копирования выполняется в отношении пользователя или для указания фирменной символики.
ms.prod: xamarin
ms.assetid: 26480465-CE19-71CD-FC7D-69D0990D05DE
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: f34a3ee44b604bf0b82faf77769f3c2844e6460f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="splash-screen"></a>Заставка

_Приложение Android занимает некоторое время для запуска, особенно при первом запуске приложения на устройстве. Экрана-заставки может отображать начала копирования выполняется в отношении пользователя или для указания фирменной символики._


## <a name="overview"></a>Обзор

Приложение Android занимает некоторое время для запуска, особенно при первом запуске приложения на устройстве (иногда это называется _холодный запуск_). Может отображать заставку запуска выполняется для пользователя, или он может отображать сведения о фирменной символике для идентификации и повышение уровня приложения.

В этом руководстве описывается один из способов реализации экрана-заставки в приложение. Он охватывает следующие шаги:

1.  Создание drawable ресурс для экрана-заставки.

2.  Определение новой темы, который будет отображать drawable ресурсов.

3.  Добавление нового действия приложения, которое будет использоваться в качестве экрана-заставки, определенные в теме, созданные на предыдущем шаге.

[![Пример Xamarin заставку с последующим экран приложения](splash-screen-images/splashscreen-01-sml.png)](splash-screen-images/splashscreen-01.png#lightbox)


## <a name="requirements"></a>Требования

В этом руководстве предполагается, что приложение обращается к Android API уровня 15 (Android 4.0.3) или более поздней версии. Приложение должно также иметь **Xamarin.Android.Support.v4** и **Xamarin.Android.Support.v7.AppCompat** пакеты NuGet, добавлены в проект.

Всего кода и XML в данном руководстве, можно найти в [экран-заставка](https://developer.xamarin.com/samples/monodroid/SplashScreen) образец проекта в этом руководстве.


## <a name="implementing-a-splash-screen"></a>Реализация экрана-заставки

Создайте пользовательскую тему и применить его к действию, выполняющего экран-заставка является самый быстрый способ отрисовки и отображение экрана-заставки. При подготовке к просмотру действия, он загружает тему и применяется к фону действия drawable ресурс (ссылка имеется в теме). Такой подход исключает необходимость создания файла разметки.

Экран-заставка реализуется как действие, которое отображает типизированной drawable, выполняет все операции инициализации и запуска всех задач. После приложение начальной загрузки, экран-заставка действия начинается основных действий и лишается стек приложения.


### <a name="creating-a-drawable-for-the-splash-screen"></a>Создание Drawable для экрана-заставки

Экран-заставка будет отображаться drawable в качестве фона экрана-заставки действия XML. Она необходима для использования точечный рисунок (например, PNG или JPG) для изображения для отображения.

В этом руководстве мы используем [списка слоев](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList) по центру изображение экрана-заставки в приложении. Ниже приведен пример `drawable` ресурсов с помощью `layer-list`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item>
    <color android:color="@color/splash_background"/>
  </item>
  <item>
    <bitmap
        android:src="@drawable/splash"
        android:tileMode="disabled"
        android:gravity="center"/>
  </item>
</layer-list>
```

Это `layer-list` будет center изображение экрана-заставки **splash.png** на фоне, указанном `@color/splash_background` ресурсов.
Поместить этот файл в **ресурсы или drawable** папку (например, **Resources/drawable/splash_screen.xml**).

После создания экрана-заставки drawable следующим шагом является создание темы для экрана-заставки.


### <a name="implementing-a-theme"></a>Реализация темы

Чтобы создать пользовательскую тему на экране-заставке действия, изменить (или добавить) файл **values/styles.xml** и создайте новый `style` элемент на экране-заставке. Образец **values/style.xml** ниже приведен файл с `style` с именем **MyTheme.Splash**:

```xml
<resources>
  <style name="MyTheme.Base" parent="Theme.AppCompat.Light">
  </style>

  <style name="MyTheme" parent="MyTheme.Base">
  </style>

  <style name="MyTheme.Splash" parent ="Theme.AppCompat.Light.NoActionBar">
    <item name="android:windowBackground">@drawable/splash_screen</item>
    <item name="android:windowNoTitle">true</item>
    <item name="android:windowFullscreen">true</item>
  </style>
</resources>
```

**MyTheme.Splash** является очень скудной &ndash; он объявляет фона окна, явно удаляет строки заголовка из окна и объявляет, что это полноэкранном режиме. Если вы хотите создать экран-заставку, эмулирующую пользовательского интерфейса приложения перед действие увеличивает разметке, можно использовать `windowContentOverlay` вместо `windowBackground` в определении стиля. В этом случае необходимо также изменить **splash_screen.xml** drawable, чтобы он отображал эмуляции пользовательского интерфейса.


### <a name="create-a-splash-activity"></a>Создание действия заставки

Теперь нам требуется новое действие для Android для запуска с нашей изображение заставки, выполняет все задачи запуска. Ниже приведен пример реализации завершения заставки экрана:

```csharp
[Activity(Theme = "@style/MyTheme.Splash", MainLauncher = true, NoHistory = true)]
public class SplashActivity : AppCompatActivity
{
    static readonly string TAG = "X:" + typeof(SplashActivity).Name;

    public override void OnCreate(Bundle savedInstanceState, PersistableBundle persistentState)
    {
        base.OnCreate(savedInstanceState, persistentState);
        Log.Debug(TAG, "SplashActivity.OnCreate");
    }

    // Launches the startup task
    protected override void OnResume()
    {
        base.OnResume();
        Task startupWork = new Task(() => { SimulateStartup(); });
        startupWork.Start();
    }

    // Simulates background work that happens behind the splash screen
    async void SimulateStartup ()
    {
        Log.Debug(TAG, "Performing some startup work that takes a bit of time.");
        await Task.Delay (8000); // Simulate a bit of startup work.
        Log.Debug(TAG, "Startup work is finished - starting MainActivity.");
        StartActivity(new Intent(Application.Context, typeof (MainActivity)));
    }
}
```

`SplashActivity` явно использует тему, который был создан в предыдущем разделе, переопределение тему по умолчанию приложения.
Нет необходимости загрузить макет в `OnCreate` как темы объявляет drawable в качестве фона.

Очень важно задать `NoHistory=true` атрибут, чтобы действие можно удалить из стека назад. Чтобы предотвратить Отмена процесса запуска кнопки "Назад", можно также переопределить `OnBackPressed` и его не выполняют никаких действий:

```csharp
public override void OnBackPressed() { }
```

При запуске работа выполняется в асинхронном режиме `OnResume`. Это необходимо, чтобы при запуске рабочего не замедлять работу или отложить внешний вид экрана запуска. После завершения работы `SplashActivity` запустит `MainActivity` и пользователь может начать работать с приложением.

Этот новый `SplashActivity` назначается операцией запуска для приложения, задав `MainLauncher` атрибут `true`. Поскольку `SplashActivity` теперь операцией запуска, необходимо изменить `MainActivity.cs`и удалите `MainLauncher` атрибута из `MainActivity`:

```csharp
[Activity(Label = "@string/ApplicationName")]
public class MainActivity : AppCompatActivity
{
    // Code omitted for brevity
}
```


## <a name="summary"></a>Сводка

В этом руководстве описано, как реализовать экран-заставку в приложении Xamarin.Android; а именно применение пользовательские темы для запуска действия.


## <a name="related-links"></a>Связанные ссылки

- [Экран-заставка (пример)](https://developer.xamarin.com/samples/monodroid/SplashScreen)
- [Drawable списка слоев](http://developer.android.com/guide/topics/resources/drawable-resource.html#LayerList)
- [ Шаблоны материала разработки - экраны запуска](https://www.google.com/design/spec/patterns/launch-screens.html)
