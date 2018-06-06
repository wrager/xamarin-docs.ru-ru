---
title: Обратная связь Haptic в Xamarin.iOS
description: В этом документе описывается haptic отзыв приложения Xamarin.iOS. Он описывает UIImpactFeedbackGenerator, UINotificationFeedbackGenerator и UISelectionFeedbackGenerator.
ms.prod: xamarin
ms.assetid: 888106D1-58F4-453F-BACC-91D51FA39C80
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: d0dae6d6f50423474fbfebad5d630000e2160f6a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790192"
---
# <a name="providing-haptic-feedback-in-xamarinios"></a>Обратная связь Haptic в Xamarin.iOS

<a name="Overview" />

## <a name="overview"></a>Обзор

На iPhone 7 и iPhone 7 плюс Apple включила haptic ответы, предоставляющие дополнительные способы физически привлекать пользователя. Haptic обратной связи (часто называют просто Haptics) использует суть сенсорного ввода (с помощью принудительного вибрации или движения) в пользовательский интерфейс конструктора. Используйте эти новые параметры tactile обратной связи для получения внимания пользователя и дополнять их действия.

Будут подробно рассмотрены следующие темы:

- [Об Haptic обратной связи](#About-Haptic-Feedback)
- [UIImpactFeedbackGenerator](#UIImpactFeedbackGenerator)
- [UINotificationFeedbackGenerator](#UINotificationFeedbackGenerator)
- [UISelectionFeedbackGenerator](#UISelectionFeedbackGenerator)

<a name="About-Haptic-Feedback" />

## <a name="about-haptic-feedback"></a>Об Haptic обратной связи

Несколько встроенных элементов пользовательского интерфейса предоставляют уже haptic отзывов, такие как средства выбора, коммутаторы и ползунков. iOS 10 теперь добавлена возможность программно триггера с помощью конкретный подкласс haptics `UIFeedbackGenerator` класса.

Разработчик может использовать один из следующих `UIFeedbackGenerator` подклассам программно триггер haptic отзыв:

- `UIImpactFeedbackGenerator` -Используйте этот генератор обратной связи в дополнение к действие или задачи, такие как представления «thud» при представлении слайдов в месте, или если два объекта на экране конфликтуют.
- `UINotificationFeedbackGenerator` -Используйте этот генератор обратной связи для уведомления, такие как завершение, со сбоем или любой другой тип предупреждения.
- `UISelectionFeedbackGenerator` -Используйте этот генератор обратной связи для выбора активно изменения, например выбора элемента из списка.

<a name="UIImpactFeedbackGenerator" />

### <a name="uiimpactfeedbackgenerator"></a>UIImpactFeedbackGenerator

Используйте этот генератор обратной связи в дополнение к действие или задачи, такие как представления «thud» при представлении слайдов в месте, или если два объекта на экране конфликтуют.

Используйте следующий код, чтобы триггер влияние отзыв:

```csharp
using UIKit;
...

// Initialize feedback
var impact = new UIImpactFeedbackGenerator (UIImpactFeedbackStyle.Heavy);
impact.Prepare ();

// Trigger feedback
impact.ImpactOccurred ();
```

Если разработчик создает новый экземпляр `UIImpactFeedbackGenerator` класса, они предоставляют `UIImpactFeedbackStyle` указание стойкости отзывы как:

- `Heavy`
- `Medium`
- `Light`

`Prepare` Метод `UIImpactFeedbackGenerator` вызывается для информирования системы, haptic отзыв собирается происходят, чтобы ее можно свести к минимуму задержки.

`ImpactOccurred` Вызывает haptic обратной связи.

<a name="UINotificationFeedbackGenerator" />

### <a name="uinotificationfeedbackgenerator"></a>UINotificationFeedbackGenerator

Используйте этот генератор обратной связи для уведомления, такие как завершение, со сбоем или любой другой тип предупреждения.

Используйте следующий код, чтобы триггер уведомления отзывов:

```csharp
using UIKit;
...

// Initialize feedback
var notification = new UINotificationFeedbackGenerator ();
notification.Prepare ();

// Trigger feedback
notification.NotificationOccurred (UINotificationFeedbackType.Error);
```

Новый экземпляр `UINotificationFeedbackGenerator` создается класс и его `Prepare` метод вызывается для информирования системы, haptic отзыв собирается происходят, чтобы ее можно свести к минимуму задержки.

`NotificationOccurred` Вызывается для запуска haptic обратную связь с данной `UINotificationFeedbackType` из:

- `Success`
- `Warning`
- `Error`

<a name="UISelectionFeedbackGenerator" />

### <a name="uiselectionfeedbackgenerator"></a>UISelectionFeedbackGenerator

Используйте этот генератор обратной связи для выбора активно изменения, например выбора элемента из списка.

Используйте следующий код для реакции на выделение триггер:

```csharp
using UIKit;
...

// Initialize feedback
var selection = new UISelectionFeedbackGenerator ();
selection.Prepare ();

// Trigger feedback
selection.SelectionChanged ();
```

Новый экземпляр `UISelectionFeedbackGenerator` создается класс и его `Prepare` метод вызывается для информирования системы, haptic отзыв собирается происходят, чтобы ее можно свести к минимуму задержки.

`SelectionChanged` Вызывает haptic обратной связи.

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован новые типы доступны в iOS 10 и способы их реализации в Xamarin.iOS haptic обратной связи.

## <a name="related-links"></a>Связанные ссылки

- [Примеры операций ввода-вывода 10](https://developer.xamarin.com/samples/ios/iOS10/)
