---
title: Класс устройства
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2017
ms.openlocfilehash: 471616dffc700cf93a9f6435565222d7628bf165
ms.sourcegitcommit: 1561c8022c3585655229a869d9ef3510bf83f00a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2018
---
# <a name="device-class"></a>Класс устройства

[ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Класс содержит несколько свойств и методов, чтобы помочь разработчикам настраивать макет и функций на отдельно для каждой платформы.

Помимо методов и свойств для целевого кода по отдельным типам оборудования и размеры `Device` класс включает [BeginInvokeOnMainThread](#Device_BeginInvokeOnMainThread) метод, который должен использоваться при взаимодействии с пользовательским Интерфейсом, элементы управления на фоновые потоки.

<a name="providing-platform-values" />

## <a name="providing-platform-specific-values"></a>Предоставление значений определяемых платформой

Перед Xamarin.Forms 2.3.4 платформы приложение было запущено на может быть получен с помощью проверки [ `Device.OS` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.OS/) свойство и его для сравнения [ `TargetPlatform.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.iOS/), [ `TargetPlatform.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Android/), [ `TargetPlatform.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.WinPhone/), и [ `TargetPlatform.Windows` ](https://developer.xamarin.com/api/field/Xamarin.Forms.TargetPlatform.Windows/) значений перечисления. Аналогично, один из [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) перегрузки может использоваться для предоставления значений от платформы к элементу управления.

Тем не менее поскольку Xamarin.Forms 2.3.4 эти API устарели и заменены. [ `Device` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/) Класс теперь содержит открытые строковые константы, определяющие платформы — [ `Device.iOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.iOS/), [ `Device.Android` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.Android/), [ `Device.WinPhone` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinPhone/) (устарело) [ `Device.WinRT` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.WinRT/) (устарело) [ `Device.UWP` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.UWP/), и [ `Device.macOS` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Device.macOS/). Аналогичным образом [ `Device.OnPlatform` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.OnPlatform/p/System.Action/System.Action/System.Action/System.Action/) перегруженные методы были заменены [ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) и [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) API-интерфейсы.

В C#, специфический для платформы значения можно задать путем создания `switch` инструкции на [ `Device.RuntimePlatform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Device.RuntimePlatform/) свойства, а затем предоставлять `case` инструкции для необходимых платформ:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) И [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) классы предоставляют те же функциональные возможности, в XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](https://developer.xamarin.com/api/type/Xamarin.Forms.OnPlatform%3CT%3E/) Класс — это универсальный класс и поэтому должны создаваться с `x:TypeArguments` атрибут, соответствующий целевой тип. В [ `On` ](https://developer.xamarin.com/api/type/Xamarin.Forms.On/) класса [ `Platform` ](https://developer.xamarin.com/api/property/Xamarin.Forms.On.Platform/) атрибут может принимать один `string` значение или несколько разделенных запятыми `string` значения.

> [!IMPORTANT]
> Предоставление с неправильным `Platform` атрибута значение в `On` класса не приведет к ошибке. Вместо этого код будет выполняться без применения значение конкретную платформу.

<a name="Device_Idiom" />

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Можно использовать для изменения схемах или функции в зависимости от приложения на устройстве. [ `TargetIdiom` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TargetIdiom/) Перечисление содержит следующие значения:

-  **Телефон** – iPhone и iPod touch и уже, чем 600 частные интерфейсы Android ^
-  **Планшет** — iPad, устройства Windows и Android шире, чем 600 частные интерфейсы ^
-  **Рабочий стол** — возвращаются только в [приложений UWP](~/xamarin-forms/platform/windows/installation/index.md) на настольных компьютерах Windows 10 (возвращает `Phone` на мобильных устройствах Windows, в том числе в сценариях континуума)
-  **ТВ** — Tizen ТВ устройств
-  **Неподдерживаемые** – не используется

*^ частные интерфейсы не обязательно является количество физических пикселей*

`Idiom` особенно удобна при создании макеты, использующие экраны больше, следующим образом:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

<a name="Device_Styles" />

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Свойство](~/xamarin-forms/user-interface/styles/index.md) содержит определения встроенного стиля, которые могут быть применены для некоторых элементов управления (таких как `Label`) `Style` свойство. Ниже приведены доступные стили.

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

<a name="Device_GetNamedSize" />

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` можно использовать при задании [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) в коде C#:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

<a name="Device_OpenUri" />

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri` Метод может использоваться для запуска операции над базовой платформой, такие как открытие URL-адрес в собственном браузере (**Safari** на iOS или **Internet** в Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[WebView образец](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) содержит пример использования `OpenUri` для открытия URL-адресов, а также активировать телефонные звонки.

[Maps образец](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) также использует `Device.OpenUri` для отображения карты и направления, используя собственные **сопоставляет** приложений на iOS и Android.

<a name="Device_StartTimer" />

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Класс также содержит `StartTimer` метод, который предоставляет простой способ для запуска задачи зависят от времени, который работает в Xamarin.Forms общий код (включая PCLs). Передайте `TimeSpan` Чтобы задать интервал и возвращать `true` для сохранения таймер или `false` остановить процесс после текущего вызова.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Если код внутри таймера взаимодействует с интерфейсом пользователя (например, вывода текста `Label` или отображение оповещение) следует выполнять в пределах `BeginInvokeOnMainThread` выражение (см. ниже).

<a name="Device_BeginInvokeOnMainThread" />

## <a name="devicebegininvokeonmainthread"></a>Device.BeginInvokeOnMainThread

Элементы пользовательского интерфейса должны быть доступны никогда не фоновых потоков, например код, выполняющийся в таймер или обработчик завершения асинхронной операции, такие как веб-запросов. Любой код фона, который необходимо обновить пользовательский интерфейс, должны быть помещены в [ `BeginInvokeOnMainThread` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Device.BeginInvokeOnMainThread/p/System.Action/). Это эквивалентно `InvokeOnMainThread` на iOS, `RunOnUiThread` в Android и `Dispatcher.RunAsync` на универсальной платформе Windows.

Используется код Xamarin.Forms.

```csharp
Device.BeginInvokeOnMainThread ( () => {
  // interact with UI elements
});
```

Обратите внимание, методы, с помощью `async/await` не обязательно должны использовать `BeginInvokeOnMainThread` при их запуске из основного потока пользовательского интерфейса.

## <a name="summary"></a>Сводка

Xamarin.Forms `Device` класс предоставляет возможность точного управления функциональные возможности и макеты отдельно для каждой платформы — даже общего кода (PCL или общие проекты).


## <a name="related-links"></a>Связанные ссылки

- [Образец устройства](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithDevice/)
- [Образец стилей](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [Устройство](https://developer.xamarin.com/api/type/Xamarin.Forms.Device/)
