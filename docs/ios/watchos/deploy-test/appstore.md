---
title: "Развертывание в магазине приложений"
description: "Развертывание приложений Контрольные значения к магазину приложения"
ms.topic: article
ms.prod: xamarin
ms.assetid: DBE16040-70D2-4F61-B5F3-C8D213DBC754
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: eed12c84b9952ef5c3dd27847071f05392bc16c4
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="deploying-to-the-app-store"></a>Развертывание в магазине приложений

> [!IMPORTANT]
>  Обязательно ознакомьтесь с [отправки Apple Watch руководства](https://developer.apple.com/app-store/watch/)и в разделе [Устранение](#Troubleshooting) проблемы, вам, возможно.

- Убедитесь, что у вас есть.
  - [**Профили подготовки распространителя** ](#provisioning) созданы для проектов.
  - **Цель развертывания** (`MinimumOSVersion`) для iOS родительского набора приложений для **8.2** или более ранней версии (не поддерживается в формате 8.3).

- В [ **iTunes Connect**](#iTunes_Connect):

  - Создание записи приложения iOS (или добавить **новую версию** существующего приложения).
  - Добавьте значок просмотра и снимки экрана.

- Затем в [Visual Studio для Mac](#xamarin_studio) (Visual Studio не поддерживается в настоящее время):

  - Щелкните правой кнопкой мыши приложение iOS и выберите **Назначить запускаемым проектом**.
  - Измените **App Store** конфигурации.
  - Используйте **архив** возможность создания архива приложения.

- Наконец, перейдите в [Xcode 6.2 +](#xcode)

  - Последовательно выберите пункты **окна > Организатор** и выберите **архивы**.
  - Выберите из списка приложений и архивирования.
  - (Необязательно) **Проверки...**  архива.
  - **Отправить...**  архива и выполните шаги, чтобы передать в iTunes Connect для проверки и утверждения.

Ознакомьтесь с советами определенные, связанные с этими ниже. В разделе [Устранение неполадок](#Troubleshooting) статьи, если возникают проблемы.

<a name="provisioning" />

## <a name="distribution-provisioning-profiles"></a>Профили подготовки распространения

Для создания приложений для магазина развертывания, необходимо создать **профиль подготовки распространения** для каждого идентификатора приложения в решении.

Если у вас есть код приложения, подстановочный знак *требуется только один профиль подготовки*; но если у вас есть отдельный идентификатор приложения для каждого проекта, то вам потребуется подготовительный профиль для каждого идентификатора приложения:

![](appstore-images/provisioningprofile-distribution-sml.png "Профиль распределения хранилище приложения")

После создания всех трех профилей, они отображаются в списке. Не забудьте загрузить и установить каждый из них (его двойным щелчком):

![](appstore-images/provisioningprofiles-sml.png "В списке доступных профилей")

Вы можете проверить профиля подготовки в **параметры проекта** , выбрав **сборки > подписывание пакета iOS** экрана и выбрав **AppStore | iPhone** Конфигурация.

**Профиль подготовки** в списке будут показаны все профили сопоставления — вы увидите сопоставления профили, созданные в этом раскрывающемся списке.

![](appstore-images/options-selectprofile-sml.png "Диалоговое окно подписывание пакета iOS")

## <a name="itunes-connect"></a>iTunes Connect

Выполните [Обзор распространения приложений](~/ios/deploy-test/app-distribution/index.md), в частности:

- [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)

При настройке приложения в iTunes Connect, не забудьте добавить значок просмотра и снимки экрана:

![](appstore-images/itunesconnect-watch-sml.png "Значок просмотра и снимки экрана в iTunes Connect")

Файл значка должен иметь 1024 x 1024 пикселей и будет иметь циклические маски, применяемый к нему, когда она появится. Значок не должно быть альфа-канала.

Хотя бы один снимок экрана является обязательным, не более пяти могут быть отправлены.
Они должны быть 312 x 390 точек и демонстрируют приложения Контрольное значение в действии.
Создавать снимки экрана с этот размер можно использовать имитатор Контрольное значение 42 мм.


<a name="xamarin_studio" />

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. Убедитесь, что приложение iOS запускаемый проект. Если нет, щелкните правой кнопкой мыши, чтобы задать для него.

  ![](appstore-images/xs-startup.png "Задание запускаемого проекта")

2. Выберите **AppStore** конфигурация построения:

  ![](appstore-images/xs-appstore.png "Конфигурации построения AppStore")

3. Выберите **сборки > архив** пункт меню, чтобы начать процесс архива:

  ![](appstore-images/xs-archive.png "Меню "Построение"")

Вы также можете **представление > архивы...**  элемента меню, чтобы увидеть архивы, которые были созданы ранее.

  ![](appstore-images/xs-archives-sml.png "Представление архивы")

<a name="xcode" />

## <a name="xcode"></a>Xcode

Xcode автоматически отобразится архивов, созданных в Visual Studio для Mac.

1. Запустить Xcode и выберите **окна > Организатор**:

  ![](appstore-images/xc-organizer.png "Меню «окно»")

2. Переключитесь в **архивы** и выберите архив, который был создан с помощью Visual Studio для Mac:

  ![](appstore-images/xc-archives.png "Вкладке архивы")

3. При необходимости **проверки...**  архива, выберите **отправить...**  к отправке приложения в iTunes Connect.

4. Выберите команды разработчиков (если они принадлежат к нескольким) и подтвердите отправки:

  ![](appstore-images/xc-submit1.png "В разделе команды разработки")

5. Посетите iTunes Connect еще раз, чтобы увидеть отправленного двоичный файл. Перейдите на страницу конфигурации приложения и выберите **предварительный выпуск** в верхнем меню, чтобы увидеть **строит** списка:

  [ ![](appstore-images/itc-prerelease-sml.png "Странице конфигурации приложения в iTunes Connect")](appstore-images/itc-prerelease.png)

Вы можете отправить приложение на утверждение на **версии** страницы. Ссылаться на [Обзор распространения приложений iOS](~/ios/deploy-test/app-distribution/index.md) для получения дополнительной информации.


## <a name="troubleshooting"></a>Устранение неполадок

Ниже приведены некоторые ошибки, которые могут возникнуть при отправке в магазине приложений, а также действия, которые можно предпринять для их исправления.

### <a name="archive-menu-option-is-not-visible-in-visual-studio-for-mac"></a>Архив меню не отображается в Visual Studio для Mac

Выполните [описанные выше действия](#xamarin_studio) Настройка решения для архивирования. Если не удается правильно установить запускаемый проект, убедитесь, что изначально устанавливается конфигурацию сборки для отладки или выпуска, прежде чем пытаться изменить запускаемый проект. Выберите конфигурацию построения обратно в **AppStore**.

### <a name="invalid-icon"></a>Значок «Недопустимо»

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/AppIcons27.5x27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Выполните [инструкциям по удалению альфа-канал](~/ios/watchos/troubleshooting.md) из значки.

### <a name="cfbundleversion-mismatch"></a>Несоответствие CFBundleVersion

```csharp
CFBundleVersion Mismatch. The CFBundleVersion value '1' of watch application
'...watchkitextension.appex/WatchApp.app' does not match the CFBundleVersion
value '1.0' of its containing iOS application `YouriOS.app`.
```

Все проекты в решении - приложение iOS, расширение контрольных значений и приложение Watch - следует использовать тот же номер версии. Изменить каждый **Info.plist** так, чтобы точно совпадает с номером версии.

### <a name="missing-icons"></a>Отсутствие значка

```csharp
Missing Icons. No icons found for watch application '...watchkitextension.appex/WatchApp.app'.
Please make sure that its Info.plist file includes entries for CFBundleIconFiles.
```

Следуйте инструкциям в [работа со значками](~/ios/watchos/app-fundamentals/icons.md) для добавления всех требуемых изображения в проект приложения контрольных значений.

### <a name="missing-icon"></a>Значок отсутствует

```csharp
Missing Icon. The watch application '...watchkitextension.appex/WatchApp.app'
is missing icon with name pattern '*44x44@2x.png' (Home Screen 42mm).
```

Обязательно последнюю версию Visual Studio для Mac и что ваш **AppIcons.appiconset** содержит полный набор образов. Если вы по-прежнему видите эту ошибку, просмотреть источник **Contents.json** для подтверждения того, он содержит запись для всех требуемых изображения. Кроме того, как только вы убедились, вы используете последнюю версию Xamarin, удалении и повторном создании **AppIcons.appiconset**.

> [!IMPORTANT]
> Примечание: Имеется известная ошибка в Visual Studio для Mac Контрольные значения значок поддержки: ожидается, что изображение 88 x 88 пикселей для  **29x29@3x**  образ (который должен иметь 87 x 87 точек).


Не удается устранить эту проблему в Visual Studio для Mac - либо изменение ресурса изображения в Xcode или вручную изменить **Contents.json** файла (для соответствия [в этом примере](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).



### <a name="invalid-watchkit-support"></a>Недопустимый WatchKit поддержки

```csharp
Invalid WatchKit Support - The bundle contains an invalid implementation of WatchKit.
The app may have been built or signed with non-compliant or pre-release tools.
```

Это сообщение может появиться во время проверки и отправки или по электронной почте автоматических из iTunes Connect после внешне успешной передачи.
<!--
Ensure you are using the latest version of Xcode and Xamarin's tools.
-->
> [!IMPORTANT]
> Примечание: Необходимо **архив** приложения в Visual Studio для Mac и затем переключитесь в Xcode 6.2 + для проверки и отправки в iTunes Connect.


Используйте канал стабильный Xamarin и Xcode 6.2 +.



### <a name="invalid-provisioning-profile"></a>Недопустимый профиль подготовки

```csharp
Invalid Provisioning Profile. The provisioning profile included in the bundle
...iOSWatchApp.watchkitapp [iOSWatchApp.app/PlugIns/...iOSWatchApp.watchkitextension.appex/WatchApp.app]
is invalid. [Missing code-signing certificate.]
```

**Профили подготовки распространителя** для всех трех проектов в решении Контрольное значение приложении должен быть предоставлен: приложение iOS, расширение контрольных значений и приложение Watch - либо явно (три профиля) или с помощью одного шаблона профиля. Проверьте существование профили подготовки в центра разработки iOS, и что вы скачали и добавить их к компьютеру Mac.

### <a name="invalid-code-signing-entitlements"></a>Недопустимый код подписи прав

```csharp
ITMS-90046: Invalid Code Signing Entitlements. Your application bundle's signature contains
code signing entitlements that are not supported on iOS. Specifically, value
'...watchkitextension' for key 'application-identifier' in '...watchkitextension'
is not supported. The value should be a string startign with your TEAMID, followed
by a dot '.' followed by the bundle identifier.
```

Убедитесь, профили подготовки, настройки правильно в центре разработчиков Apple, и что вы скачали и установлены. Также проверьте, что они установлены в Visual Studio для Mac в окне Свойства для каждого проекта.

### <a name="invalid-architecture"></a>Недопустимая архитектура

```csharp
Invalid architecture: Apps that include an app extension
and framework must support arm64.
```

Контрольное значение приложения можно добавлять только [единой API (64-разрядная версия)](~/cross-platform/macios/unified/index.md) Xamarin.iOS приложений.
Правой кнопкой мыши проект приложения iOS, а затем перейдите к **параметры > сборки > сборка iOS > вкладка «Дополнительно»** и убедитесь, что **поддерживаемых архитектур** для AppStore iPhone конфигурация включает **ARM64** (например) **ARMv7 + ARM64**).

### <a name="this-bundle-is-invalid"></a>Этот пакет является недопустимым.

```csharp
ITMS-90068: This bundle is invalid. The value provided for the key
MinimumOSVersion '8.3' is not acceptable.
```

Приложение iOS родительского должно иметь MinimumOSVersion, значение «8.2"или выше.

### <a name="non-public-api-usage"></a>Использование API неоткрытых

```csharp
Your app contains non-public API usage.
Please review the errors, and resubmit your application.
```

Убедитесь, что вы используете последнюю версию средства Xcode и Xamarin.
Код не должен обращаться к интерфейсы API не являющиеся открытыми.

### <a name="build-error-mt5309"></a>Построение MT5309 ошибки

```csharp
Error MT5309: Native linking error: clang: error: no such file or directory:
```

Эта ошибка, скорее всего, результат на переименован Xcode установку из **Xcode.app**. Например, эта ошибка возникает при переименовании установку, чтобы **XCode 6.2.app**.



## <a name="related-links"></a>Связанные ссылки

- [Руководство по отправке WatchKit Apple](https://developer.apple.com/app-store/watch/)
