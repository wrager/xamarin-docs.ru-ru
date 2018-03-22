---
title: Apple Pay
description: "В этом руководстве описывается настройка Xamarin.iOS среды для использования с Apple Pay платить за физических товаров, например пищевых продуктов, развлечения и членства в приложении. Он содержит сведения о необходимых идентификаторы, сертификаты и прав."
ms.topic: article
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af899bb1c5708e3fc0be88db6224d9127f5a5c6d
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="apple-pay"></a>Apple Pay

_В этом руководстве описывается настройка Xamarin.iOS среды для использования с Apple Pay платить за физических товаров, например пищевых продуктов, развлечения и членства в приложении. Он содержит сведения о необходимых идентификаторы, сертификаты и прав._


Apple Pay была введена вместе с iOS 8, позволяя пользователям оплатить физические товары, например пищевых продуктов, развлечения и членство через свои устройства iOS. Его можно найти на iPhone 6 и iPhone 6 плюс, а также может быть связан с Apple Watch для покупок в магазине. При использовании на iPhone, он использует Touch ID, чтобы подтвердить и авторизовать транзакции пользователя кредитной или дебетовой картой.


## <a name="requirements"></a>Требования

Apple Pay доступен только в пределах iOS 8 и более поздних версий и поэтому требуется по крайней мере Xcode 6.

Необходимы также следующие элементы для интеграции Apple Pay в приложении.

 - Процессор оплаты
 - Идентификатор продавца
 - Сертификат Apple Pay
 - Apple Pay правах

В этом документе будут рассмотрены более подробно эти элементы.

## <a name="differences-between-apple-pay-and-iap"></a>Различия между Apple оплаты и Отзывах

Основное различие между Apple Pay и *приобретения в приложении* (Отзывах) относится к продуктов, которые они продают. *Физический* продаже товаров через Apple Pay; пищевых продуктов, услуги размещения и физических развлечения (таких как билеты кинофильмов) являются все примеры. Напротив, продает Отзывах *виртуальных* товаров, таких как premium или дополнительное содержимое и дополнительных месяцев подписки — задержек потоковой службы или дополнительных сроков в игру.

Платформы, которые используются также являются ключевое различие; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) — используется для оплаты Apple, а [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) предоставляет платформу API для Отзывах.

С Apple Pay Apple [состояния](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , он «не плату пользователей, предпринимателей или разработчиков Apple оплаты для платежей». В отличие от этого Отзывах имеет плата 30% для каждой транзакции. Кроме того, с Apple Pay, транзакции не проходит через Apple вообще, вместо этого он проходит через платформа платежей.


## <a name="using-a-payment-processor-platform"></a>С помощью платформы процессора оплаты

Одна из основных частей Apple Pay — обработки платежей. Хотя существует возможность сделать это самостоятельно, требуются значительные знания криптографии
- как описано в компании Apple [обработки платежа руководства](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Платформы обработки платежей, с другой стороны, обрабатывать эти операции для вас, позволяя сосредоточиться на построении приложения.

Две следующие варианты:

- **Stripe** -Зарегистрируйтесь на [Stripe.com](https://stripe.com/) для доступа к их интерфейсов API.

- **JudoPay** -извлечь их [Xamarin образец кода на github](https://github.com/Judopay/Xamarin-Sample-App)и при регистрации [JudoPay.com](https://www.judopay.com/).


## <a name="provisioning-for-apple-pay"></a>Подготовка для оплаты Apple

Настройка приложения для использования Apple Pay требует установки на портале разработчиков Apple и в приложении. Существует ряд действий, которые следует выполнить для успешной инициализации приложения для Apple оплаты:

1. Создайте идентификатор продавца:
    - Выполните действия, [здесь](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Создайте идентификатор приложения с возможностью применения оплаты и добавление продавца.
    - Выполните действия, [здесь](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Создание сертификата для идентификатора Merchant:
    - Выполните действия, [здесь](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Создайте профиль подготовки с только что созданный идентификатор приложения:
    - Выполните действия, [здесь](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Добавьте Apple Pay прав:
    - Выберите право оплаты Apple описанных [здесь](~/ios/deploy-test/provisioning/entitlements.md), или вручную добавить в файл из пары ключ значение [здесь](~/ios/deploy-test/provisioning/entitlements.md)


## <a name="working-with-apple-pay"></a>Работа с Apple оплаты

Apple iOS 10 разрешить пользователю безопасный выплатах с веб-сайтов и взаимодействии с Siri и карты предприняла ряд улучшений платить Apple.

С iOS 10 ряд новых интерфейсов API были добавлены, работающие с iOS и watchOS для поддержки динамического оплаты сетей и среде тестирования новой "песочницы".


### <a name="apple-pay-website-integration"></a>Интеграция веб-сайте Apple оплаты

Новые для iOS 10 разработчику возможность включать Apple Pay непосредственно в своих веб-сайтов с помощью **ApplePay JS**. Пользователи, просмотр веб-сайта с помощью Safari в iOS или macOS могут вносить платежи с Apple Pay путем проверки транзакции на iPhone или Apple Watch. Дополнительные сведения см. в разделе Apple [ApplePay JP Framework ссылка](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Усовершенствования PassKit Framework

В iOS 10 PassKit framework была расширена для поддержки Apple платить за пределами `UIKit` и разрешить издателей карты для представления собственные карты в своих приложениях.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Поддержка Apple оплаты за пределами UIKit

С помощью [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) и [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), приложение может поддерживать одинаковые функциональные возможности, предоставляемые [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) без использования UIKit. Хотя этот новый API для поддержки Apple Pay Apple Watch (и в конкретных целей, а также), он является необязательным в других ситуациях (например, существующие приложения). Тем не менее Apple рекомендует как можно быстрее перемещение в новый интерфейс API для поддержки широкого Apple Pay во всех приложениях разработчика с единой базой кода. Дополнительные сведения о цели и Siri интеграции, см. в разделе нашей [введение в SiriKit](~/ios/platform/sirikit/index.md) документации.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Представления карты издателя из приложений

С iOS 10 были добавлены новые функции для платформы PassKit, которое разрешить издателей карты для представления их карты своих приложений. Разработчик может добавить `PKPaymentButtonTypeInStore` UIButton для приложения пользовательский интерфейс, который будет отображать кнопку Apple Pay для карты.

`PresentPaymentPass` Метод [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) класс может также использоваться для отображения карты программными средствами.

### <a name="new-payment-network-support"></a>Поддержка новых сетевых оплаты

Новое в iOS 10, приложение может автоматически поддерживают новую сеть оплаты когда он станет доступен без необходимости изменения, перекомпиляции приложения и повторно отправить его в магазин приложений разработчика.

Новый [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) метод `PKPaymentNetwork` класса позволяет приложению, для обнаружения сети, доступные на устройстве пользователя во время выполнения. Кроме того [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) свойство была расширена вступили в качестве аргумента имя поставщика платежа. С помощью этих методов, приложение можно автоматически поддерживают все сети, которая поддерживает поставщика платежа.

Дополнительные сведения см. в разделе нашей [конфигурации платить Apple](~/ios/platform/apple-pay.md) и Apple [руководстве платить Apple](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Новая среда тестирования

С iOS 10 Apple реализовала новую среду тестирования, которая позволяет разработчику для подготовки карты платежа теста непосредственно на устройстве iOS. Этот новый тестовой среды затем возвращается оплаты зашифрованные тестовых данных приложения.

Чтобы включить новую среду тестирования, выполните следующее:

1. Создайте новый тестирования iCloud учетной записи в iTunes Connect.
2. Войдите на устройство iOS с новой учетной записи для тестирования.
3. Задайте нужный регион для тестирования приложения.
4. Используйте один из карты платежа теста из [руководстве платить Apple](https://developer.apple.com/apple-pay/) для выполнения платежей.

> [!IMPORTANT]
> С помощью переключения iCloud учетные записи, устройство автоматически переключается в новую среду тестирования. Однако по-прежнему Apple **требует** приложение, чтобы протестировать с реального карты в рабочей среде перед отправкой в магазин приложений iTunes.

## <a name="summary"></a>Сводка

В этой статье мы анализировать различные элементы, необходимые для использования в приложении Apple Pay. Мы рассмотрели создание идентификатор продавца и способах ее использования в пределах **Entitlements.plist**, который необходимо изменить вручную.


## <a name="related-links"></a>Связанные ссылки

- [Покупки из приложений](~/ios/platform/in-app-purchasing/index.md)
- [Введение в PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (пример)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
