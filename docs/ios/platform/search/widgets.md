---
title: "Поиска и улучшения мини-приложение на начальный экран"
description: "В этой статье рассматриваются усовершенствования, внесенные в систему мини-приложения в iOS 10 Apple."
ms.topic: article
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a6749ca9d8a793372ec088433780d622f2f05b41
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>Поиска и улучшения мини-приложение на начальный экран

_В этой статье рассматриваются усовершенствования, внесенные в систему мини-приложения в iOS 10 Apple._


Apple был представлен ряд дополнительных возможностей для системы мини-приложения, чтобы гарантировать, что удобна в мини-приложений на фоновые, имеющиеся на новый iOS 10 экран блокировки. Кроме того, теперь содержат мини-приложения [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) свойство, которое позволяет разработчику для описания объем содержимого доступен и пользователь может разворачивать и сворачивать содержимое.

Мини-приложения (также известный как сегодня расширения) — это специальный тип iOS расширения, который отображать небольшой объем полезной информации и предоставления функциональных возможностей конкретного приложения в течение отведенного времени. Например, новостей приложения имеет мини-приложения, отображающего top заголовки и календарь приложения предоставляет два различных мини-приложения: одно для отображения современных события и одно — чтобы показывать предстоящих событий.

Широкие возможности настройки мини-приложения и может содержать элементы пользовательского интерфейса, такие как текст, изображения, кнопки, и т. д. Кроме того разработчик может настроить макета их мини-приложения.

[ ![](widgets-images/widgets01.png "Пример мини-приложения")](widgets-images/widgets01.png)

Существует два основных областей, которые пользователь может просматривать и взаимодействовать с мини-приложения, приложения:

- **Экран поиска** -пользователи могут добавлять мини-приложения, они считают наиболее полезными для их экран поиска. Экран поиска осуществляется путем считывания вправо на экранах домашней и блокировки.
- **Начального экрана** -с начального экрана, пользователь может использовать 3D Touch для открытия списка быстрых действий, применяя давление значка приложения. Мини-приложения, приложение будет отображаться над краткий список действий. См. в разделе нашей [введение в 3D Touch](~/ios/platform/3d-touch.md) Дополнительные сведения см.

## <a name="widgets-developer-suggestions"></a>Предложения разработчика мини-приложения

В идеальном случае разработчик повторите и должны создавать средства визуализации, которые пользователь желает добавить экраны поиска. С этой целью Apple имеет следующие рекомендации:

- **Создать лучшие, взгляда качества** -пользователя требуется мини-приложений, которые взгляда краткую информацию обновления состояния или разрешить им быстро выполнять простые задачи. Таким образом, предоставляя нужный объем информации и интерактивные возможности необходимое. Когда это возможно, позволяют пользователю для выполнения данной задачи одним касанием. Кроме того поскольку мини-приложения не поддерживают панорамирования и прокрутки, это будет принимать во внимание при разработке мини-приложения.
- **Показать содержимое быстро** -мини-приложения разработаны с учетом взгляда, поэтому пользователь никогда не должен иметь ожидания содержимое для загрузки после отображения мини-приложения. Мини-приложений следует кэшировать их содержимое локально, поэтому они могут показывать последнего содержимого во время загрузки нового содержимого в фоновом режиме.
- **Укажите соответствующие поля и поля** -мини-приложения никогда не должны выглядеть центр, поэтому Избегайте расширение содержимое по границам представления мини-приложения. Всегда должно быть несколько расширенных точках между границей и содержимым. Apple также предлагает использовать значок приложения, в верхней части мини-приложения, в качестве руководства для выравнивания. Если мини-приложение представляет макет сетки, убедитесь, что соответствующие заполнение между элементами в сетке и попробуйте ограничить число элементов на четыре max.
- **Использовать адаптируемые макеты** -ширина мини-приложения будет различаются в зависимости от устройства, его выполнения и ориентации устройства. Высота мини-приложения можно также различаются в зависимости от если он отображается в Collapsed (по умолчанию) или разреженный (поддерживаются не все мини-приложения). Свернуть мини-приложение имеет высоту строки таблицы примерно два с половиной стандартных операций ввода-вывода. Разработчик может запросить размер мини-приложение развернуто, но она в идеале должна быть меньше, чем высота экрана. В состоянии Collapsed мини-приложения должны показывать автономного только нужные сведения. Если разреженный, мини-приложения должны показывать дополнительные сведения, позволяющие повысить основного содержимого, представленная в виде Collapsed. Мини-приложений, перечисленных в списке быстрого действие будет находиться в состоянии Collapsed.
- **Не Настройка фона графического элемента** -мини-приложения отображаются на фоне света, размытое, предоставляемые системой. Это делается для обеспечения согласованности между мини-приложения и улучшения читаемости их содержимого. Избегайте использования изображения фоновым мини-приложение, так как он может пересекаться с фоновых блокировки и начального экрана пользователя.
- **Использовать системный шрифт в черный или темно-серый** — при отображении текста в мини-приложении, лучше системного шрифта. Черный или темно серым цветом оформить фоне света, размытое мини-приложение следует шрифт.
- **Включения приложений доступ при соответствующих** -мини-приложения всегда должны работать отдельно из своего приложения, тем не менее, если требуется более глубокого функциональность, мини-приложения требуется возможность запустить приложение, чтобы просмотреть или изменить конкретные сведения. Просто никогда не включать кнопка «откройте приложение» позволяет пользователю коснитесь само содержимое, и никогда не открытые стороннего приложения.
- **Выберите команду Clear краткое имя мини-приложение** -над содержимым мини-приложение всегда отображается значок приложения и имя мини-приложения. Apple предлагает, использовать имя приложения для его основной мини-приложения и понятный и краткое имя для других предоставляемых им. При предоставлении пользовательский заголовок мини-приложения, должны иметь префикс с именем приложения (например, карты рядом, Maps ресторанов, и т. д.).
- **Сообщить, если проверка подлинности добавляет значение** — Если дополнительных функций или данных доступен, только если пользователь проходит проверку подлинности и вход, этот пользователь получает. Например расстояния, приложение для управления доступом my say «При входе в дороге книги».
- **Выберите элемент списка быстрого действия** -Если приложение предоставляет несколько мини-приложений, разработчик должен выберите то, что для представления, когда пользователь вызывает краткий список действий, применяя давление значок приложения с помощью 3D Touch.

Дополнительные сведения о работе с мини-приложений см. в разделе нашей [введение расширения](~/ios/platform/extensions.md), [введение в 3D Touch](~/ios/platform/3d-touch.md) документация и Apple [приложения расширения руководство по программированию](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Работа с естественность

Естественность гарантирует, что текст графического элемента остается читаемым, когда появится индикатор мини-приложение, размытой фоновой (предоставляемые системой). До iOS 10, разработчик может использовать [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) для естественность мини-приложения. Пример:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

Это в iOS 10 устаревшим и должны быть заменены с помощью [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) или [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Пример:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Работа с свернутым и развернутым мини-приложения

Новые для iOS 10 мини-приложения содержат [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) свойство, которое позволяет разработчику для описания объем содержимого доступен и пользователь может разворачивать и сворачивать содержимое.

Когда изначально отображается мини-приложения, он находится в состоянии свернуто. Свернуть мини-приложение имеет высоту строки таблицы примерно два с половиной стандартных операций ввода-вывода. Разработчик может запросить размер мини-приложение развернуто, но она в идеале должна быть меньше, чем высота экрана. 

В состоянии Collapsed мини-приложения должны показывать автономного только нужные сведения. Если разреженный, мини-приложения должны показывать дополнительные сведения, позволяющие повысить основного содержимого, представленная в виде Collapsed. Например приложение погоды показывает текущий погодных условий при сворачивании и добавляет каждый час, прогноз раскрываться.

Мини-приложений, перечисленных в списке быстрого действие будет находиться в состоянии Collapsed. Если приложение содержит более одного мини-приложения, разработчик следует выбрать один для представления, когда пользователь вызывает краткий список действий, применяя давление значок приложения с помощью 3D Touch.

Ниже приведен простого сегодня расширения (мини-приложения), обрабатывающий Collapsed и раскрываются состояния:

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

Взгляните на конкретный код режим отображения мини-приложение подробно. Чтобы сообщить системе о том, что это мини-приложение поддерживает состояние разреженный, ее использует:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Чтобы получить текущий режим отображения мини-приложение, в нем используются.

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Чтобы получить максимальный размер для состояния Expanded или Collapsed, он использует:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

И для обработки изменение состояния (режим отображения), он использует:

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

Кроме настройки запрошенный размер для каждого состояния (Collapsed или разреженный), он также обновляет содержимого, отображаемого для соответствия новому размеру.

## <a name="summary"></a>Сводка

В этой статье описаны усовершенствования, внесенные в систему мини-приложения в iOS 10 Apple, а также показан способ реализации их в Xamarin.iOS.



## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
- [Введение в расширения](~/ios/platform/extensions.md)
- [Общие сведения о 3D Touch](~/ios/platform/3d-touch.md)
- [Руководство по программированию расширения приложения](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
