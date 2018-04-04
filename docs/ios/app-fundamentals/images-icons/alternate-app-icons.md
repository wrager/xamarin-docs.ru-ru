---
title: Значки альтернативный приложений
description: В этой статье описан с помощью значков альтернативный приложения Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 302fa818-33b9-4ea1-ab63-0b2cb312299a
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/29/2017
ms.openlocfilehash: 8d9f27d58a881878aabeda4326805eec726c247c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="alternate-app-icons"></a>Значки альтернативный приложений

_В этой статье описан с помощью значков альтернативный приложения Xamarin.iOS._

Apple добавлен ряд дополнительных возможностей iOS 10.3 разрешить приложение для управления ее значок:

 - `ApplicationIconBadgeNumber` — Возвращает или задает эмблема значка приложения в Springboard.
 - `SupportsAlternateIcons` -Если `true` приложение имеет дополнительный набор значков.
 - `AlternateIconName` — Возвращает имя выбранного в данный момент альтернативный значка или `null` при использовании первичного значок.
 - `SetAlternameIconName` -Используйте этот метод переключиться на альтернативного значок данного значка приложения.

![](alternate-app-icons-images/icons04.png "Пример оповещения, когда приложение изменяет его значок")

<a name="Adding-Alternate-Icons" />

## <a name="adding-alternate-icons-to-a-xamarinios-project"></a>Добавление в проект Xamarin.iOS альтернативный значков

Чтобы переключиться на альтернативного значок приложения, должны быть включены в проект приложения Xamarin.iOS потребуется коллекцию изображения значков. Эти образы невозможно добавить к проекту, используя стандартное `Assets.xcassets` метода, они должны быть добавлены **ресурсов** непосредственно в папку.

Выполните следующие действия:

1. Выберите значок "обязательно" образы в папке, выберите все и перетащите их в **ресурсов** папки в **обозревателе решений**:

    ![](alternate-app-icons-images/icons00.png "Выбор изображения значков из папки")

2. При появлении запроса выберите **копирования**, **использовать те же действия для всех выбранных файлов** и нажмите кнопку **ОК** кнопки:

    ![](alternate-app-icons-images/icons02.png "Добавить файл в папке-диалоговое окно")

3. **Ресурсов** папки должна выглядеть после завершения следующим образом:

    ![](alternate-app-icons-images/icons01.png "Папка Resources должен выглядеть следующим образом")

<a name="Modifying-the-Info.plist-File" />

## <a name="modifying-the-infoplist-file"></a>Изменение файла Info.plist

Добавлены необходимые образами **ресурсы** папки, [CFBundleAlternateIcons](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CoreFoundationKeys.html#//apple_ref/doc/uid/TP40009249-SW13) ключ будет необходимо добавить в проект **Info.plist** файла. Этот ключ будет определять имя нового значка и изображения, составляющих его.

Выполните следующие действия:

1. В **Обозревателе решений** дважды щелкните файл **Info.plist**, чтобы открыть его для редактирования.
2. Переключитесь в **источника** представления.
3. Добавить **объединить значки** ключа и оставить **тип** значение **словарь**.
4. Добавить `CFBundleAlternateIcons` ключа и установить **тип** для **словарь**.
5. Добавить `AppIcons2` ключа и установить **тип** для **словарь**. Это будет имя нового набора значков альтернативный приложения.
6. Добавить `CFBundleIconFiles` ключа и установить **тип** для **массива**
7. Добавить новую строку в `CFBundleIconFiles` массива для каждого файла значка, оставляя расширения и `@2x`, `@3x`, суффиксы и т.п. (пример `100_icon`). Повторите этот шаг для каждого файла, содержащийся в наборе альтернативный значков.
8. Добавить `UIPrerenderedIcon` ключа для `AppIcons2` словарь, набор **тип** для **логическое** и значение для **нет**.
9. Сохраните изменения в файле.

Итоговый **Info.plist** файл должен выглядеть после завершения следующим образом:

![](alternate-app-icons-images/icons03.png "Полный файл Info.plist")

Или сделайте так, если открыть в текстовом редакторе.

```xml
<key>CFBundleIcons</key>
<dict>
    <key>CFBundleAlternateIcons</key>
    <dict>
        <key>AppIcon2</key>
        <dict>
            <key>CFBundleIconFiles</key>
            <array>
                <string>100_icon</string>
                <string>114_icon</string>
                <string>120_icon</string>
                <string>144_icon</string>
                <string>152_icon</string>
                <string>167_icon</string>
                <string>180_icon</string>
                <string>29_icon</string>
                <string>40_icon</string>
                <string>50_icon</string>
                <string>512_icon</string>
                <string>57_icon</string>
                <string>58_icon</string>
                <string>72_icon</string>
                <string>76_icon</string>
                <string>80_icon</string>
                <string>87_icon</string>
            </array>
            <key>UIPrerenderedIcon</key>
            <false/>
        </dict>
    </dict>
</dict>
```

<a name="Managing-the-Apps-Icon" />

## <a name="managing-the-apps-icon"></a>Управление значок приложения 

Значок изображения, включенный в проект Xamarin.iOS и **Info.plist** файл правильно настроены, разработчик может использовать один из многих операций ввода-вывода 10.3 добавлены новые возможности для управления значка приложения.

`SupportsAlternateIcons` Свойство `UIApplication` класса позволяет разработчику для просмотра, если приложение поддерживает альтернативный значков. Пример:

```csharp
// Can the app select a different icon?
PrimaryIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
AlternateIconButton.Enabled = UIApplication.SharedApplication.SupportsAlternateIcons;
```

`ApplicationIconBadgeNumber` Свойство `UIApplication` класса позволяет разработчику получить или задать в Springboard текущее количество эмблема значка приложения. Значение по умолчанию равно нулю (0). Пример:

```csharp
// Set the badge number to 1
UIApplication.SharedApplication.ApplicationIconBadgeNumber = 1;
```

`AlternateIconName` Свойство `UIApplication` класса позволяет разработчику для получения имени значок выбранный альтернативный приложений или возвращается `null` Если приложение использует первичный значок. Пример:

```csharp
// Get the name of the currently selected alternate
// icon set
var name = UIApplication.SharedApplication.AlternateIconName;

if (name != null ) {
    // Do something with the name
}
```

`SetAlternameIconName` Свойство `UIApplication` класса позволяет разработчику для изменения значка приложения. Передайте имя значка, чтобы выбрать или `null` чтобы вернуться на первичный значок. Пример:

```csharp
partial void UsePrimaryIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName (null, (err) => {
        Console.WriteLine ("Set Primary Icon: {0}", err);
    });
}

partial void UseAlternateIcon (Foundation.NSObject sender)
{
    UIApplication.SharedApplication.SetAlternateIconName ("AppIcon2", (err) => {
        Console.WriteLine ("Set Alternate Icon: {0}", err);
    });
}
```

При запуске приложения и пользователя выбрать альтернативный значок, отобразится предупреждение следующим образом:

![](alternate-app-icons-images/icons04.png "Пример оповещения, когда приложение изменяет его значок")

При переключении обратно в основной значок будет отображаться предупреждение следующим образом:

![](alternate-app-icons-images/icons05.png "Пример оповещения при изменении приложения на значок источника")

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье рассматривается добавление значков приложения альтернативный проект Xamarin.iOS и с их помощью внутри приложения.



## <a name="related-links"></a>Связанные ссылки

- [Образец iOSTenThree](https://developer.xamarin.com/samples/ios/iOS10/iOSTenThree)
