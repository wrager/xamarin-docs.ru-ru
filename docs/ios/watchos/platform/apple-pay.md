---
title: Apple Pay на watchOS
description: В этой статье рассматриваются усовершенствования Apple внес в Apple Pay в watchOS 3 и способы их реализации в Xamarin.iOS для Apple Watch.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: b46a0e57ea9abc5c4ec4fc2aba1e6940249b64fb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="apple-pay-on-watchos"></a>Apple Pay на watchOS

Apple внесла ряд улучшений Apple Pay в watchOS 3 добавлена поддержка платежей в приложении. Это позволяет пользователям безопасно предоставить оплаты и контактных данных для оплаты физических товаров и услуг непосредственно из Apple Watch.


## <a name="about-apple-pay-enhancements"></a>Об улучшениях оплаты Apple

Как Stated выше Apple предприняла ряд усовершенствований для Apple Pay watchOS 3 обеспечивает безопасный оплаты и контактные данные для оплаты физических товаров и услуг непосредственно из Apple Watch. Эти усовершенствования обеспечиваются с помощью изменения PassKit framework.

С iOS 10 и watchOS 3 ряд новых интерфейсов API были добавлены, работающие с iOS и watchOS для поддержки динамического оплаты сетей и среде тестирования новой "песочницы".

## <a name="passkit-framework-enhancements"></a>Усовершенствования PassKit Framework

В iOS 10 PassKit framework была расширена для поддержки Apple платить за пределами `UIKit` и разрешить издателей карты для представления из карты в своих приложениях. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Поддержка Apple оплаты за пределами UIKit

С помощью [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) и [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), приложение может поддерживать одинаковые функциональные возможности, предоставляемые [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) без использования UIKit. Хотя этот новый API для поддержки Apple Pay Apple Watch (и в конкретных целей, а также), он является необязательным в других ситуациях (например, существующие приложения). Тем не менее Apple рекомендует как можно быстрее перемещение в новый интерфейс API для поддержки широкого Apple Pay во всех приложениях разработчика с единой базой кода. Дополнительные сведения о цели и Siri интеграции, см. в разделе нашей [введение в SiriKit](~/ios/platform/sirikit/index.md) документации.

### <a name="presenting-issuer-cards-from-within-apps"></a>Представления карты издателя из приложений

С iOS 10 и watchOS 3 были добавлены новые функции для платформы PassKit, которое разрешить карты издателей представить свои оплаты карты в своих приложений. Разработчик может добавить `PKPaymentButtonTypeInStore` UIButton для приложения пользовательский интерфейс, который будет отображать кнопку Apple Pay для карты.

`PresentPaymentPass` Метод [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) класс может также использоваться для отображения карты программными средствами.

## <a name="new-payment-network-support"></a>Поддержка новых сетевых оплаты

Новые iOS 10 и watchOS 3, приложение может автоматически поддерживают новую сеть оплаты при их поступления без необходимости изменения, перекомпиляции приложения и повторно отправить его в магазин приложений разработчика.

Новый [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) метод `PKPaymentNetwork` класса позволяет приложению, для обнаружения сети, доступные на устройстве пользователя во время выполнения. Кроме того [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) свойство была расширена вступили в качестве аргумента имя поставщика платежа. С помощью этих методов, приложение можно автоматически поддерживают все сети, которая поддерживает поставщика платежа.

Дополнительные сведения см. в разделе нашей [конфигурации платить Apple](~/ios/platform/apple-pay.md) и Apple [руководстве платить Apple](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Новая среда тестирования

С iOS 10 и watchOS 3 Apple реализовала новую среду тестирования, которая позволяет разработчику для подготовки карты платежа теста непосредственно на устройстве iOS. Этот новый тестовой среды затем возвращается оплаты зашифрованные тестовых данных приложения.

Чтобы включить новую среду тестирования, выполните следующее:

1. Создайте новый тестирования iCloud учетной записи в iTunes Connect.
2. Войдите на устройство iOS с новой учетной записи для тестирования.
3. Задайте нужный регион для тестирования приложения.
4. Используйте один из карты платежа теста из [руководстве платить Apple](https://developer.apple.com/apple-pay/) для выполнения платежей.

> [!NOTE]
> С помощью переключения iCloud учетные записи, устройство автоматически переключается в новую среду тестирования. Однако по-прежнему Apple **требует** приложение, чтобы протестировать с реального карты в рабочей среде перед отправкой в магазин приложений iTunes.

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован усовершенствования Apple внес в Apple Pay в watchOS 3 и способы их реализации в Xamarin.iOS.
