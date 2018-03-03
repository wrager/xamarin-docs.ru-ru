---
title: "Принимающая Xamarin.Android широковещательных пакетов"
description: "В этом разделе описывается использование приемником широковещательных пакетов."
ms.topic: article
ms.prod: xamarin
ms.assetid: B2727160-12F2-43EE-84B5-0B15C8FCF4BD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 7ea280e11e4051b51da8c20eb9ac5a4e298547f3
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="broadcast-receiver-in-xamarinandroid"></a>Принимающая Xamarin.Android широковещательных пакетов

_В этом разделе описывается использование приемником широковещательных пакетов._


## <a name="broadcast-receiver-overview"></a>Общие сведения о широковещательных получателя

Объект _широковещательных получателя_ — это компонент Android, который позволяет приложению реагировать на сообщения (Android [ `Intent` ](https://developer.xamarin.com/api/type/Android.Content.Intent/)), рассылаются Android операционной системой или приложением. Выполните широковещательных рассылок _публикации подписки_ модели &ndash; событие вызывает вещания должны публиковаться и полученных те компоненты, которые нужны в событии. 

Android идентифицирует вещание на две категории:

* **Обычный вещания** &ndash; обычный вещания будут направляться на всех зарегистрированных широковещательных получателей в неопределенном порядке. Каждый получатель получит намерение неопределенном порядке. 
* **Упорядоченные broadcast** &ndash; упорядоченный вещания доставляется по одному зарегистрированных получателям. При получении намерение широковещательных получателя можно изменить назначение или оно может завершаться вещания.

Широковещательный получатель является подклассом `BroadcastReceiver` класс и его необходимо переопределить [ `OnReceive` ](https://developer.xamarin.com/api/member/Android.Content.BroadcastReceiver.OnReceive/p/Android.Content.Context/Android.Content.Intent/) метод. Будет выполняться Android `OnReceive` в основном потоке, поэтому этот метод должны разрабатываться выполняться быстро. Следует соблюдать осторожность при создании процесса потоков в `OnReceive` так, как Android может завершить процесс при завершении метода. Если широковещательных приемника выполнить долго выполняемые действия, то рекомендуется запланировать _задания_ с помощью `JobScheduler` или _диспетчера заданий Firebase_. Планирование работы с заданием обсуждаются отдельные руководства.

_Намерения фильтр_ используется для регистрации приемником широковещательной рассылки, чтобы Android правильной передачи сообщений. Блокировка с намерением фильтра можно указать во время выполнения (это иногда называется _контекст зарегистрирован получателя_ или как _динамическую регистрацию_) или можно статически определить в Android ( манифеста_зарегистрированные манифеста получателя_). Xamarin.Android предоставляет атрибут C# `IntentFilterAttribute`, статически, зарегистрирует намерения фильтр (это обсуждаются более подробно далее в этом руководстве). 

Основное различие между получатель зарегистрированные манифеста и получателя контекст зарегистрирован — что получателя контекст зарегистрирован будет отвечать только на широковещательные рассылки во время выполнения приложения во время регистрации манифеста получателя может отвечать на осуществляет широковещательную рассылку, даже если приложение не запущено.  

Существует два набора API-интерфейсы для управления широковещательных приемника и отправке широковещательного:

1. **`Context`** &ndash; `Android.Content.Context` Класс может использоваться для регистрации широковещательных приемник, который будет отвечать на системные события. `Context` Также используется для публикации широковещательных рассылок во всей системе.
2. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; API-интерфейс, который доступен через [пакет NuGet v4 библиотека поддержки Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Этот класс используется для сохранения широковещательных рассылок и широковещательных получателей, изолированных в контексте приложения, которое работает с ними. Этот класс может быть полезно для предотвращения отвечать на широковещательные рассылки только для приложений и отправки сообщений частной получателям других приложений.

Широковещательных приемник может отображаться диалоговые окна и его настоятельно не рекомендуется для запуска действия из вещания получателя. Если широковещательных получатель должен уведомить пользователя, его следует опубликовать уведомление.

Не поддерживается для привязки к или запустить службу в широковещательных получателя. 

В этом руководстве рассматривается создание широковещательных приемника и зарегистрировать его, чтобы она может получать широковещательные пакеты.

## <a name="creating-a-broadcast-receiver"></a>Создание приемника вещания

Создание широковещательных получателя Xamarin.Android, приложение должно подкласс `BroadcastReceiver` класса, его с оформления `BroadcastReceiverAttribute`и Переопределите `OnReceive` метод:

```csharp
[BroadcastReceiver(Enabled = true, Exported = false)]
public class SampleReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here.
        
        String value = intent.GetStringExtra("key");
    }
}
```

Когда Xamarin.Android компилирует класса, он также обновить AndroidManifest с meta данными, необходимыми для регистрации в приемник. Для широковещательных приемника статически зарегистрированы `Enabled` правильно, должно быть присвоено `true`, в противном случае Android не смогут создавать экземпляры приемника.
 
`Exported` Свойство управляет ли широковещательных получатель может получать сообщения извне приложения. Если свойство не задано явно, значение по умолчанию свойства определяется Android на основе, при наличии фильтры цель, связанные с широковещательных получателя. Если имеется хотя бы одного фильтра назначение для широковещательной рассылки получателя, а затем Android предполагается, что `Exported` свойство `true`. Если не намерение-фильтры, связанные с широковещательных получателем, то Android будет предполагать, что значение является `false`. 

`OnReceive` Метод получает ссылку на `Intent` , была подготовлена широковещательных приемник. Это делает возможна отправитель назначение для передачи значений широковещательных приемник.

### <a name="statically-registering-a-broadcast-receiver-with-an-intent-filter"></a>Статически регистрация широковещательных приемника с намерением фильтра

Когда `BroadcastReceiver` снабжен [ `IntentFilterAttribute` ](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/), Xamarin.Android добавит необходимые `<intent-filter>` элемент Android манифеста во время компиляции. Следующий фрагмент кода приведен пример широковещательных приемника, который будет выполняться после завершения загрузки (если пользователю были предоставлены соответствующие разрешения Android) устройства.

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { Android.Content.Intent.ActionBootCompleted })]
public class MyBootReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Work that should be done when the device boots.     
    }
}
```

Можно также создание условий фильтра, который будет реагировать на настраиваемые цели. Рассмотрим следующий пример. 

```csharp
[BroadcastReceiver(Enabled = true)]
[IntentFilter(new[] { "com.xamarin.example.TEST" })]
public class MySampleBroadcastReceiver : BroadcastReceiver
{
    public override void OnReceive(Context context, Intent intent)
    {
        // Do stuff here
    }
}
```

### <a name="context-registering-a-broadcast-receiver"></a>Регистрация контекста широковещательных получателя 

Регистрации контекста приемника выполняется путем вызова `RegisterReceiver` метод и широковещательной рассылки получателя должны быть отменена с помощью вызова `UnregisterReceiver` метод. Чтобы предотвратить происходит утечка ресурсов, важно отменять регистрацию получателя, если он больше не связано с контекстом. Например служба может широковещательных пакетов целью для информирования действие, которое обновления становятся доступными для отображения пользователю. При запуске действия будет зарегистрирован для этих целей. Когда действие перемещается в фоновом режиме и больше не отображается для пользователя, его необходимо отменить регистрацию получателя, так как пользовательский Интерфейс для отображения обновлений больше не отображается. В следующем фрагменте кода приведен пример того, как регистрировать и отменять регистрацию широковещательных получателя в контексте действия:

```csharp
[Activity(Label = "MainActivity", MainLauncher = true, Icon = "@mipmap/icon")]
public class MainActivity: Activity 
{
    MySampleBroadcastReceiver receiver;
    
    protected override void OnCreate(Bundle savedInstanceState)
    {
        base.OnCreate(savedInstanceState);
        receiver = new MySampleBroadcastReceiver()
        
        // Code omitted for clarity
    }
    
    protected override OnResume() 
    {
        base.OnResume();
        RegisterReceiver(receiver, new IntentFilter("com.xamarin.example.TEST"));
        // Code omitted for clarity
    }
    
    protected override OnPause() 
    {
        UnregisterReceiver(receiver);
        // Code omitted for clarity
        base.OnPause();
    }
}
```

В предыдущем примере, когда действие поступает на переднем плане, будет зарегистрирован широковещательных приемник, который будет прослушивать пользовательских назначение с помощью `OnResume` метод жизненного цикла. Когда действие переносится в фоновом режиме, `OnPause()` метод будет отменена регистрация приемника.

## <a name="publishing-a-broadcast"></a>Публикация вещания

Широковещательный опубликована, инкапсулируя _действия_ в назначение и распределения их с помощью одного из двух API: 

1. **[`LocalBroadcastManager`](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))** &ndash; Целей, которые публикуются с `LocalBroadcastManager` будет получена только приложения; они не будут направляться на другие приложения. Это должен быть предпочтительно, как он обеспечивает дополнительный уровень безопасности за счет целей в текущем приложении, и так как все находится в процессе нет не издержки, связанные с вызовов между процессами. Этот фрагмент кода показывает, как действие может отправлять намерения использование `LocalBroadcastManager`:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   Android.Support.V4.Content.LocalBroadcastManager.GetInstance(this).SendBroadcast(message);
   ```

2. **[`Context.SendBroadcast`](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/) методы** &ndash; существует несколько реализаций этого метода.
   Эти методы будут разосланы цель для всей системы. Это обеспечивает большую гибкость, но также означает, что другие приложения могут зарегистрироваться для получения приложения. Это может представлять угрозу безопасности. Приложения может потребоваться реализовать добавление безопасности для обеспечения несанкционированный доступ к цель. Этот фрагмент кода является одним из примеров того, как отправлять цели, используя один из `SendBroadcast` методов:

   ```csharp
   Intent message = new Intent("com.xamarin.example.TEST");
   // If desired, pass some values to the broadcast receiver.
   intent.PutExtra("key", "value");
   SendBroadcast(intent);
   ```
        
> [!NOTE]
> LocalBroadcastManager доступна через [v4 библиотека поддержки Xamarin](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) пакет NuGet. 

В этом фрагменте еще один пример отправки вещания с помощью `Intent.SetAction` метод для идентификации действия:

```csharp 
Intent intent = new Intent();
intent.SetAction("com.xamarin.example.TEST");
intent.PutExtra("key", "value");
SendBroadcast(intent);
```



## <a name="related-links"></a>Связанные ссылки

- [BroadcastReceiver](https://developer.xamarin.com/api/type/Android.Content.BroadcastReceiver/)
- [Context.RegisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.RegisterReceiver/p/Android.Content.BroadcastReceiver/Android.Content.IntentFilter/System.String/Android.OS.Handler/)
- [Context.SendBroadcast](https://developer.xamarin.com/api/member/Android.Content.Context.SendBroadcast/p/Android.Content.Intent/)
- [Context.UnregisterReceiver](https://developer.xamarin.com/api/member/Android.Content.Context.UnregisterReceiver/p/Android.Content.BroadcastReceiver/)
- [Intent](https://developer.xamarin.com/api/type/Android.Content.Intent/)
- [IntentFilter](https://developer.xamarin.com/api/type/Android.App.IntentFilterAttribute/)
- [LocalBroadcastManager](https://developer.android.com/reference/android/support/v4/content/LocalBroadcastManager.html#sendBroadcast(android.content.Intent))
- [Локальный уведомления в Android](~/android/app-fundamentals/notifications/local-notifications.md)
- [Android поддерживает библиотеку v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)
