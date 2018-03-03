---
title: "Изменения платформы дополнительных iOS 9"
description: "В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих платформ IOS 9."
ms.topic: article
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 82c6c2451deafb8a4314254a8138804d927c9bbf
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="additional-ios-9-frameworks-changes"></a>Изменения платформы дополнительных iOS 9

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих платформ IOS 9._

[ ![](additional-framework-changes-images/ios9-sml.png "iOS 9 эмблемы")](additional-framework-changes-images/ios9.png)

Помимо основных изменений для операций ввода-вывода Apple внес изменения и усовершенствования несколько существующих инфраструктур в iOS 9.

## <a name="av-foundation-framework-additions"></a>Дополнения Framework AV Foundation

В платформе AV Foundation [AVSpeechSynthesisVoice](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechSynthesisVoice/) класс теперь позволяет указать голос по идентификатору Помимо языка.

Например следующий код возвращает список всех доступных голоса:

```csharp
var voices = AVSpeechSynthesisVoice.GetSpeechVoices ();
```

Затем можно использовать один из голосов, в списке, установив его как `Voice` свойства экземпляра [AVSpeachUtterance](https://developer.xamarin.com/api/type/AVFoundation.AVSpeechUtterance/) класса.

[AVQueuePlayer](https://developer.xamarin.com/api/type/AVFoundation.AVQueuePlayer/) класс теперь поддерживает смешанные Интернет потоковой передачи и на основе файлов мультимедиа в очереди. Предыдущие версии может только очереди media того же типа.

Дополнительные сведения см. в разделе Apple [AVSpeechSynthesisVoice ссылка](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVSpeechSynthesisVoice_Ref/index.html#//apple_ref/occ/cl/AVSpeechSynthesisVoice).

## <a name="avkit-framework-additions"></a>Дополнения AVKit Framework

Для работы с новой функцией рисунок в изображение (PIP), содержащий новый AVKit framework `AVPictureInPictureController` и [AVPlayerViewController](https://developer.xamarin.com/api/type/AVKit.AVPlayerViewController/) классы:

- **AVPictureInPictureController** -этот класс позволяет приложению iOS 9 реагировать на пользователя, запуск воспроизведения видео в окне PIP с плавающей запятой, размер которой можно изменять на iPad.
- **AVPlayerViewController** -управляет `AVPlayer` контроллер, используемый для представления видео в окне PIP с плавающей запятой, размер которой можно изменять на iPad.

Дополнительные сведения см. в разделе нашей [многозадачность для iPad](~/ios/platform/introduction-to-ios9/index.md#multitasking) документация и Apple [ссылки AVPictureInPictureController](https://developer.apple.com/library/prerelease/ios/documentation/AVKit/Reference/AVPictureInPictureController_Class/index.html#//apple_ref/occ/cl/AVPictureInPictureController) и [ссылки AVPlayerViewController](https://developer.apple.com/library/prerelease/ios/documentation/AVFoundation/Reference/AVPlayerViewController_Class/index.html#//apple_ref/occ/cl/AVPlayerViewController).

## <a name="introducing-cloudkit-web-services"></a>Знакомство с CloudKit веб-служб

CloudKit framework упрощает разработку приложений, iCloud доступ. Сюда входят методы для получения данных приложений и средств права, а также возможность безопасного хранения сведений о приложении. Этот комплект предоставляет пользователям слоя анонимности, разрешив доступ к приложениям с их идентификаторами iCloud без совместного использования личных сведений.

Новый _CloudKit веб-службы_ framework предоставляет библиотека JavaScript (CloudKit JS), могут быть внедрены в веб-сайта для предоставления доступа к тому же CloudKit на основе данных и содержимое в виде приложения Xamarin.iOS.

> [!IMPORTANT]
> **Примечание:** перед доступ, представления или обновление содержимого из базы данных CloudKit, используя CloudKit JS, должен определенным ранее схеме этой базы данных.




Дополнительные сведения см. в разделе следующие документы:

- [Общие сведения о CloudKit](~/ios/data-cloud/intro-to-cloudkit.md) -наши Общие сведения об использовании CloudKit в приложении Xamarin.iOS.
- [Быстрый запуск CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloudKitQuickStart/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014987) -введение CloudKit компании Apple.
- [Справочник по JS CloudKit](https://developer.apple.com/library/prerelease/ios/documentation/CloudKitJS/Reference/CloudKitJavaScriptReference/index.html#//apple_ref/doc/uid/TP40015359) -CloudKit JS документации компании Apple.
- [CloudKit веб-службы ссылки](https://developer.apple.com/library/prerelease/ios/documentation/DataManagement/Conceptual/CloutKitWebServicesReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40015240) -Apple ссылку, которая описывает интерфейс для CloudKit HTTP.
- [Каталог CloudKit: Введение в CloudKit (Cocoa и JavaScript)](https://developer.apple.com/library/prerelease/ios/samplecode/CloudAtlas/Introduction/Intro.html#//apple_ref/doc/uid/TP40014599) -Apple примера приложения, с помощью CloudKit и CloudKit JS.

## <a name="foundation-framework-additions"></a>Дополнения Framework Foundation

Apple включены следующие изменения в платформе Foundation в iOS 9:

### <a name="changes-to-nsbundle"></a>Изменения в NSBundle

Следующие изменения были внесены в [NSBundle](https://developer.xamarin.com/api/type/Foundation.NSBundle/) класс IOS 9:

* `GetPreservationPriorityForTag (NSString tag)` — Возвращает текущий приоритет сохранение ресурсов с указанным тегом. Допустимые значения находятся в диапазоне от `0.0` для `1.0`, сначала очистки ресурсов с более низким приоритетом.
* `SetPreservationPriorityForTag (double priority, NSSet tags)` — Задает текущий приоритет сохранение за ресурсы с заданной тегов. Допустимые значения находятся в диапазоне от `0.0` для `1.0`, сначала очистки ресурсов с более низким приоритетом.

Дополнительные сведения см. в разделе Apple [NSBundle ссылка](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSBundle_Class/index.html#//apple_ref/occ/cl/NSBundle).

### <a name="changes-to-nsprocessinfo"></a>Изменения в NSProcessInfo

Каждый процесс, запущенный на устройстве iOS — один, _процесс агента сведения_ (PIA). Используйте [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) класса для предоставления сведений о текущей основных сборках ВЗАИМОДЕЙСТВИЯ и управления питания и охлаждения управления для данного процесса.

Например для автоматического завершения процесса управления можно использовать следующий код:

```csharp
// Disable automatic termination
var activity = NSProcessInfo.ProcessInfo.BeginActivity(NSActivityOptions.AutomaticTerminationDisabled, "Define reason for change here...");

// Perform the required task
...

// Return to normal operation
NSProcessInfo.ProcessInfo.EndActivity(activity);
```

Дополнительные сведения см. в разделе Apple [NSProcessInfo ссылка](https://developer.apple.com/library/prerelease/ios/documentation/Cocoa/Reference/Foundation/Classes/NSProcessInfo_Class/index.html#//apple_ref/occ/cl/NSProcessInfo).

### <a name="reacting-to-low-power-mode"></a>Отклик на режим низкого потребления

Используйте `LowPowerModeEnabled` свойство [NSProcessInfo](https://developer.xamarin.com/api/type/Foundation.NSProcessInfo/) класс, чтобы определить, включен ли на устройстве iOS, приложение выполняется в режим низкого потребления. Пример:

```csharp
// Is the device in low power mode?
if (NSProcessInfo.ProcessInfo.LowPowerModeEnabled) {
    // Reduce activity to conserve energy...
} else {
    // Return to normal activity...
}
```

## <a name="healthkit-framework-changes"></a>Изменения HealthKit Framework

Apple включены следующие изменения в [HealthKit](https://developer.xamarin.com/api/namespace/HealthKit/) framework в iOS 9:

- Поддержка массовое удаление и отслеживания удаления записей в базе данных HealthKit. В разделе Apple [HKDeletedObject](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKDeletedObject_ClassReference/index.html#//apple_ref/occ/cl/HKDeletedObject), [HKAnchoredObjectQuery](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKAnchoredObjectQuery_Class/index.html#//apple_ref/occ/cl/HKAnchoredObjectQuery) и [HKHealthStore ссылку на класс](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HKHealthStore_Class/index.html#//apple_ref/doc/uid/TP40014708) для получения дополнительной информации.
- Новые категории отслеживания и характеристики были добавлены `HKQuantityTypeIdentifier` класса (такие как `UVExposure`) и `HKCategoryTypeIdentifier` класса (например, `OvulationTestResult`). В разделе Apple [HealthKit константы ссылку](https://developer.apple.com/library/prerelease/ios/documentation/HealthKit/Reference/HealthKit_Constants/index.html#//apple_ref/doc/uid/TP40014710) Дополнительные сведения.

См. в нашем [введение в HealthKit](~/ios/platform/healthkit.md) Дополнительные сведения о работе с HealthKit в Xamarin.iOS.

## <a name="local-authentication-framework-changes"></a>Изменения Framework локальной проверки подлинности

Apple включены следующие изменения в [локальной проверки подлинности](https://developer.xamarin.com/api/namespace/LocalAuthentication/) framework в iOS 9:

- С помощью `EvaluateAccessControl` и `EvaluatePolicy` методы [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) класса, после этого можно повторно использовать Touch ID соответствий из предыдущей успешной разблокировки попыток.
- Возможность получить список зарегистрированных в настоящее время пальцами.
- Поддержка отслеживания, когда палец добавляется или удаляется из проверки подлинности.
- Возможность использования _контекст проверки подлинности_ в цепочке ключей вызовов и поддержки по оценке доступ к цепочке ключей управления списками.
- Возможность отменить запрос пользователя из кода.

См. в нашем [введение в Touch ID](~/ios/platform/touchid.md) Дополнительные сведения о работе с Touch ID в Xamarin.iOS.

### <a name="lacontext-changes"></a>LAContext изменения

Следующие изменения были внесены в [LAContext](https://developer.xamarin.com/api/type/LocalAuthentication.LAContext/) класс IOS 9:

- **TouchIdAuthenticationMaximumAllowableReuseDuration** -возвращает максимальный объем времени, который можно использовать проверку подлинности touch ID.
- **EvaluatedPolicyDomainState** — Возвращает или задает состояние вычисления политики.
- **MaxBiometryFailures** -больше поддерживаются в iOS 9.
- **TouchIdAuthenticationAllowableReuseDuration** Возвращает или задает временной интервал, который можно использовать проверку подлинности touch ID.
- **EvaluateAccessControl** — асинхронно оценивает политику проверки подлинности.
- **Сделать недействительным** -проверку подлинности данного touch ID делает недействительными.
- **IsCredentialSet** -возвращает `true` Если учетных данных, заданных в настоящее время.
- **SetCredentialType** задает указанный тип учетных данных.

См. в разделе Apple [LAContext ссылки](https://developer.apple.com/library/prerelease/ios/documentation/LocalAuthentication/Reference/LAContext_Class/index.html#//apple_ref/occ/instm/LAContext/evaluatePolicy:localizedReason:reply:) для получения дополнительных сведений.

## <a name="mapkit-framework-changes"></a>Изменения MapKit Framework

Apple включены следующие изменения в [MapKit](https://developer.xamarin.com/api/namespace/MapKit/) framework в iOS 9:

- MapKit теперь обеспечивает поддержку для запуска приложения карты непосредственно в направлениях пути и для выполнения запросов к передаче предполагаемое время прибытия (эта) с помощью [MKLaunchOptions](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) и [MKDirections](https://developer.xamarin.com/api/type/MapKit.MKLaunchOptions/) классы.
- Результаты поиска, возвращенный MapKit и [CLGeocoder](https://developer.xamarin.com/api/type/CoreLocation.CLGeocoder/) класс также может предоставлять результат часового пояса.
- Теперь можно полностью настроить карты заметки, представленные на приложения iOS с помощью `DetailCalloutAccessoryView` свойство [MKAnnotationView](https://developer.xamarin.com/api/type/MapKit.MKAnnotationView/) класса.

См. в разделе нашей [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) и [Пошаговое руководство. Просмотр заметок и перекрытия в MapKit](~/ios/user-interface/controls/ios-maps/ios-maps-walkthrough.md) Дополнительные сведения о работе с картами и заметок в Xamarin.iOS и Apple [Ссылки CLGeocoder](https://developer.apple.com/library/prerelease/ios/documentation/CoreLocation/Reference/CLGeocoder_class/index.html#//apple_ref/occ/cl/CLGeocoder) для получения дополнительной информации.

## <a name="passkit-framework-additions"></a>Дополнения PassKit Framework

Apple включены следующие изменения в [PassKit](https://developer.xamarin.com/api/namespace/PassKit/) framework в iOS 9:

- Apple Pay теперь поддерживает хранилище дебета и кредитных карт вместе с Discover карты. В разделе **сетей оплаты** раздел Apple [PKPaymentRequest ссылку на класс](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKPaymentRequest_Ref/index.html#//apple_ref/doc/uid/TP40014832) Дополнительные сведения.
- Из непосредственно в приложении Xamarin.iOS, теперь можно добавить сетей оплаты и издателей карты для оплаты Apple. В разделе Apple [PKAddPaymentPassViewController ссылку на класс](https://developer.apple.com/library/prerelease/ios/documentation/PassKit/Reference/PKAddPaymentPassViewController_Class/index.html#//apple_ref/doc/uid/TP40016116) для получения дополнительных сведений.

См. в нашем [введение в PassKit](~/ios/platform/passkit.md) Дополнительные сведения о работе с PassKit в Xamarin.iOS.

## <a name="safari-services-framework-additions"></a>Safari служб Framework дополнения

Apple включены следующие изменения в [служб Safari](https://developer.xamarin.com/api/namespace/SafariServices/) framework в iOS 9:

- Теперь вы можете использовать новый [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) класса для отображения веб-содержимого в приложении Xamarin.iOS. Он позволяет совместно использовать данные веб-сайта и использовании файлов cookie с приложением Safari, а также некоторые функции, Safari (например, средства чтения и Автозаполнение). [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) функции **сделать** кнопки, которая будет возвращать пользователей в приложение при завершении работы, просмотра веб-содержимого.

Поскольку [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) класс предназначен для отображения на одной странице веб-содержимого, можно использовать его для замены всех [WKWebKit](https://developer.xamarin.com/api/type/WebKit.WKWebView/) или [UIWebView](https://developer.xamarin.com/api/type/UIKit.UIWebView/)элементов управления в существующие приложения Xamarin.iOS.

### <a name="displaying-a-website"></a>Отображение веб-сайта

В следующем примере кода приведен пример вызова [SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) из внутри другого представления контроллера:

```csharp
// Create an instance of the Safari Services View Controller
var controller = new SFSafariViewController(new NSUrl("http://www.xamarin.com"));

// Display website
PresentViewController(controller, true, null);
```

## <a name="uikit-framework-changes"></a>Изменения UIKit Framework

Apple включила усовершенствованиях несколько элементов [UIKit](https://developer.xamarin.com/api/namespace/UIKit/) framework IOS 9. В следующих разделах будут подробно эти изменения.

### <a name="3d-touch-events"></a>События 3D Touch

Новый iOS 9 и iPhone 6s и iPhone 6s Plus, 3D Touch добавляет давление конфиденциальные жестов в приложения iOS. В результате, если приложение запущено на iOS 9 (или более поздней), поддерживающих 3D Touch способно устройство iOS внесение изменений в нехватки приведет к `TouchesMoved` вызова события. 

Из-за этого изменения в поведении приложения iOS должно быть подготовлено для `TouchesMoved` событий для вызова более часто, даже в том случае, если X / Y координаты не изменились. 

Дополнительные сведения см. в разделе нашей [введение в 3D Touch](~/ios/platform/3d-touch.md) руководства.

### <a name="document-open-in-place-functionality"></a>Функциональные возможности Open на месте документа

С помощью `FinishedLaunching (application, launchOptions)` или `WillFinishLaunching (Application, launchOptions)` методы [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) класса, теперь можно открыть документ и измените его на месте (в отличие от работы на копии).

Для поддержки новых функциональных возможностей открытым на месте, добавьте `LSSupportsOpeningDocumentsInPlace` ключа для приложения Xamarin.iOS **Info.plist** файл со значением `YES`.

См. в разделе Apple [UIApplicationDelegate ссылки](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) для получения дополнительных сведений.

### <a name="enhanced-touch-events"></a>События расширенного сенсорного экрана

Apple обеспечивает ряд дополнительных возможностей к событиям касания в iOS 9. К ним относятся возможность использовать Touch прогноза и получить доступ к промежуточного штрихи между операциями обновления отображения.

См. в разделе Apple [руководство обработки событий для операций ввода-вывода](https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Introduction/Introduction.html) Дополнительные сведения.

### <a name="fetching-tailored-content"></a>Выборка специально настроенные содержимого

Новый `NSDataAsset` класс позволяет приложения Xamarin.iOS для выборки содержимого, адаптированные к памяти и графических возможностей, которая запущена на устройстве iOS.

### <a name="new-layout-anchors"></a>Новый макет привязки

Новый `NSLayoutAnchor` и `NSLayoutDimension` работают классы привязки макета с помощью новых свойств привязки [UIView](https://developer.xamarin.com/api/type/UIKit.UIView/) класса (например, `LeadingAnchor` и `WidthAnchor`) для упрощения макета в iOS 9.

См. в разделе нашей [введение в единой раскадровки](~/ios/user-interface/storyboards/unified-storyboards.md) Дополнительные сведения о работе с AutoLayout и размер классов в приложения Xamarin.iOS и Apple [NSLayoutAnchor ссылки](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutAnchor_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutAnchor), [ Справочник по NSLayoutDimension](https://developer.apple.com/library/prerelease/ios/documentation/AppKit/Reference/NSLayoutDimension_ClassReference/index.html#//apple_ref/occ/cl/NSLayoutDimension) и [UIView ссылки](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIView_Class/index.html#//apple_ref/occ/cl/UIView) Дополнительные сведения.

### <a name="new-readable-content-margins"></a>Новые поля для чтения содержимого

Новый `UILayoutGuide` класс может использоваться для предоставления полей для чтения содержимого и определить области рисования содержимого внутри представления. В разделе Apple [UILayoutGuide ссылки](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UILayoutGuide_Class_Reference/index.html#//apple_ref/occ/cl/UILayoutGuide) Дополнительные сведения.

### <a name="text-input-in-notifications-modifications"></a>Ввод текста в изменения уведомления

[UIUserNotificationAction](https://developer.xamarin.com/api/type/UIKit.UIUserNotificationAction/) класс имеет новый `Behavior` свойство, которое может использоваться для поддержки введенный текст уведомления.

### <a name="uiapplicationdelegate-changes"></a>UIApplicationDelegate изменения

Хотя формально не устарела, Apple, они предлагают замена всех вызовов `FinishedLaunching (UIApplication application)` метод [UIApplicationDelegate](https://developer.xamarin.com/api/type/UIKit.UIApplicationDelegate/) класса либо `FinishedLaunching (UIApplication application, NSDictionary launchOptions)` или `WillFinishLaunching (UIApplication application, NSDictionary launchOptions)` методы.

См. в разделе Apple [UIApplicationDelegate ссылки](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationDelegate_Protocol/index.html#//apple_ref/occ/intf/UIApplicationDelegate) для получения дополнительных сведений.

### <a name="uikit-dynamics-changes"></a>Изменения Dynamics UIKit

Apple включены следующие изменения в UIKit Dynamics в iOS 9:

- Dynamics теперь поддерживает границы непрямоугольные конфликтов.
- Новый настраиваемый `UIFieldBehavior` класс используется для поддержки различных типов полей.
- Были добавлены дополнительные вложения типы `UIAttachmentBehavior` класса.

См. в разделе Apple [UIAttachment ссылки](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIAttachmentBehavior_Class/index.html#//apple_ref/occ/cl/UIAttachmentBehavior) для получения дополнительных сведений.

### <a name="uipickerview-and-uidatepicker-changes"></a>UIPickerView и UIDatePicker изменений

До iOS 9 [UIPickerView](https://developer.xamarin.com/api/type/UIKit.UIPickerView/) и [UIDatePicker](https://developer.xamarin.com/api/type/UIKit.UIDatePicker/) элементы управления были неизменяемый и будет автоматически изменяться размеры до ширины контейнера (обычно ширину устройства iOS, приложение ОС).

В iOS 9 это автоматическое изменение размеров больше не происходит и элементы управления будут отображены в ширину 320 точки на всех устройствах iOS, независимо от размера экрана и ориентацию.

Для исправления ситуации, используйте автоматический макет и размер классы закрепить ширину элемента управления по границам родительского контейнера (представление), а также укажите требуемую высоту. См. в разделе нашей [введение в единой раскадровки](~/ios/user-interface/storyboards/unified-storyboards.md) Дополнительные сведения о работе с автоматический макет и размер классов в приложении Xamarin.iOS.

### <a name="new-uitextinputassistantitem-class"></a>Новый класс UITextInputAssistantItem

Используйте новую `UITextInputAssistantItem` класса группы кнопок панели макета в _панель_. На панели инструментов является новой области, доступной в Экранная клавиатура для предоставления ввода клавиш.



## <a name="related-links"></a>Связанные ссылки

- [Образцы iOS 9](https://developer.xamarin.com/samples/ios/iOS9/)
- [Введение в iOS 9](~/ios/platform/introduction-to-ios9/index.md)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [Новые возможности iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
