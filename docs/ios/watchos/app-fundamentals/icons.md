---
title: "Работа со значками"
ms.topic: article
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 98cd780a29abdbeaab02483e4b6ed01a218f88e5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-icons"></a>Работа со значками

Apple Watch решений требуется два набора значков:

* Значки приложения iOS, которые будут отображаться на iPhone.
* Apple Watch значки, которые будут передаваться в кружке в меню "Контрольные значения" и на экранах уведомления. Также отображается значок приложения в [Apple Watch](~/ios/watchos/app-fundamentals/settings.md) приложения iOS.

## <a name="apple-watch-icons"></a>Значки Apple Watch

<table align="center" border="1" cellpadding="1" cellspacing="1">
    <tr>
      <td valign="top">
        <b>Значок приложения iOS</b>
      </td>
      <td valign="top">
Отображается на iPhone и запускает родительского приложения </td>
      <td>
        <img src="icons-images/icon-ios.png" class="tableimg">
      </td>
    </tr>
    <tr>
      <td valign="top" rowspan="3">
        <b>Значок приложения</b>
      </td>
      <td valign="top">
Отображается на начальном экране Apple Watch </td>
      <td>
        <img src="icons-images/icon-home.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
Отображается на уведомления Контрольное значение </td>
      <td>
        <img src="icons-images/notification-icon.png" class="tableimg" />
      </td>
    </tr>
    <tr>
      <td valign="top">
Отображается в <a href="~/ios/watchos/app-fundamentals/settings.md">Apple Watch приложения iOS</a>
      </td>
      <td>
        <a href="icons-images/watch-app.png">
          <img src="icons-images/watch-app-sml.png" class="tableimg">
        </a>
      </td>
    </tr>
    <tbody>
</table>



## <a name="configuring-your-solution"></a>Настройка решения

Чтобы убедиться, что ваше приложение iOS и приложение watch Показать правильное имя и значок, выполните следующие действия для каждого проекта:

### <a name="ios-app"></a>Приложение iOS

Ссылаться на [руководства значков приложения iOS](~/ios/app-fundamentals/images-icons/app-icons.md) чтобы значки приложения iOS настроены правильно.

#### <a name="infoplist"></a>Info.plist

Строка, которая появляется рядом с приложения в системе наблюдения за [параметры приложения Apple Watch](~/ios/watchos/app-fundamentals/settings.md) настраивается в **Info.plist приложения iOS**.

Убедитесь, что ваш **Info.plist** имеет `CFBundleName` ключ и значение (Примечание: Это отличается от `CFBundleDisplayName`, может иметь оба):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Приложения для Apple Watch

Один раз на [родительского приложения](~/ios/watchos/app-fundamentals/parent-app.md) настроил значки, необходимо добавить в каталоге активов значок приложения в приложение "Контрольные значения".

1. Правой кнопкой мыши проект приложения контрольных значений и выберите **файл > Добавить > новый файл... > iOS > каталога активов** для добавления в проект в каталоге активов.

 ![](icons-images/newasset.png "Добавьте в проект в каталоге активов")

2. Дважды щелкните **AppIcons.appiconset/Contents.json** файла

  ![](icons-images/xcassets-iconset-sml.png "Содержимое AppIcons")

3. Добавьте все образы watchOS, как показано на этом снимке экрана.

  [![](icons-images/appicons-sml.png "Добавьте все изображения watchOS, как показано на этом снимке экрана")](icons-images/appicons.png#lightbox)

  Ссылаться на [рекомендации Apple значок](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) для необходимые размеры (размеры также отображаются на экране). Помните, автоматически обрезаются эти значки для подготовки к просмотру в кружке.

  Список значок должен выглядеть примерно следующим образом:

  ![](icons-images/xcassets-complete-sml.png "Список значков в обозревателе решений")

4. Для обеспечения каталога активов включается в приложении, добавьте следующий раздел и значение **Info.plist приложение Watch**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Можно проверить значки настроены правильно, проверьте [Apple Watch параметры приложения](~/ios/watchos/app-fundamentals/settings.md) в iPhone на симуляторе или создания [уведомления](~/ios/watchos/platform/notifications.md) и подтверждения значок отображается на уведомления экран.

> [!NOTE]
> **Примечание**: значки не может иметь альфа-канала (приложения будут отклонены во время отправки в магазин приложений при наличии альфа-канала). Можно проверить, существует ли альфа-канал и удалите его [с помощью Предварительный просмотр приложения на Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Связанные ссылки

- [Значок & изображений Apple руководства](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
