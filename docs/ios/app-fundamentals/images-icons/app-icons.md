---
title: "Значков приложения"
description: "Это статье рассматриваются включая и управлении ресурса изображения в приложении Xamarin.iOS для использования в качестве значка приложения."
ms.topic: article
ms.prod: xamarin
ms.assetid: B7791574-4A0F-4CB6-8C18-36D40B5C91EB
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 05/22/2017
ms.openlocfilehash: df9059b0e64b4a05b554f25b5f9d7f6031406633
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="application-icons"></a>Значков приложения

_Это статье рассматриваются включая и управлении ресурса изображения в приложении Xamarin.iOS для использования в качестве значка приложения._

Будут подробно рассмотрены следующие темы:

* [Приложения, Spotlight и параметры значков](#icon-types) -различные типы значков, необходимые для приложения iOS.
* [Управление значки с каталогами активов](#managing) — управление значков приложения с помощью средств каталоги.
* [iTunes иллюстрации](#itunes) -предоставление необходимых iTunes иллюстрации Ad-Hoc метода доставки приложения.

<a name="icon-types" />

## <a name="application-spotlight-and-settings-icons"></a>Приложения, Spotlight и параметры значков

Таким же образом, приложения Xamarin.iOS можно использовать ресурсы изображений для элементов управления пользовательского интерфейса и в качестве значков документов активы изображений можно использовать для предоставления значков приложения. Следующие снимки экрана из iPad показано три использует значков в iOS.

- **Значок приложения** -каждого приложения iOS необходимо определить значка приложения. Данный значок, предоставляющую пользователю из на начальный экран операций ввода-вывода, чтобы запустить приложение. Кроме того этот значок используется Game Center, если это применимо. Пример 

    [![](app-icons-images/000.png "Значок приложения")](app-icons-images/000-full.png#lightbox)
- **Значок Spotlight** — всякий раз, когда пользователь вводит имя приложения в поиске внимания, отображается этот значок. Пример 

    [![](app-icons-images/000a.png "Значок Spotlight")](app-icons-images/000a-full.png#lightbox)
- **Значок параметров** — Если пользователь вводит **параметры** приложение на устройстве iOS, данный значок будет отображаться в конце **параметры** список для приложения. Пример 

    [![](app-icons-images/000b.png "Значок параметров")](app-icons-images/000b-full.png#lightbox)

Для поддержки всех типов значок, необходимые для приложения Xamarin.iOS разрабатывать приложения для iOS 5 через iOS 9 (или более поздней) потребуется следующие размеры изображения средства и способы их устранения:

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPhone</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 и 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 и 8</b></td>
        <td align="center" bgcolor="#F9F9F9"><b>iOS 9 и 10<b><br/><i>(iPhone 6 и 7, а также)</i></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Тип значка</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>3x</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Значок приложения</td>
        <td align="center">57x57</td>
        <td align="center">114x114</td>
        <td align="center" style="color:#BBBBBB;">60x60<sup>(1)</sup></td>
        <td align="center">120x120</td>
        <td align="center">180x180</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Spotlight</td>
        <td align="center">29x29</td>
        <td align="center">58x58</td>
        <td align="center" style="color:#BBBBBB;">40x40<sup>(2)</sup></td>
        <td align="center">80x80</td>
        <td align="center">120x120</td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Параметры</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(4)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(4)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center">87x87</td>
    </tr>
</table>

<table cellpadding="7" cellspacing="0" width="100%">
    <tr valign="top">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="5" align="center" bgcolor="#F0F0F0"><b>iPad</b></td>
    </tr>
    <tr valign="center">
        <td width="200" style="border-width: 0px;"></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 5 и 6</b></td>
        <td colspan="2" align="center" bgcolor="#F9F9F9"><b>iOS 7 и 8</b></td>
        <td colspan="1" align="center" bgcolor="#F9F9F9"><b>iOS&nbsp;9 и 10</b></td>
    </tr>
    <tr valign="top" bgcolor="#F0F0F0">
        <td width="200" align="center"><b>Тип значка</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>1x</b></td>
        <td align="center"><b>2x</b></td>
        <td align="center"><b>2x<br/>iPad&nbsp;Pro</b></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Значок приложения</td>
        <td align="center">72x72</td>
        <td align="center">144x144</td>
        <td align="center">76x76</td>
        <td align="center">152x152</td>
        <td align="center">167x167<sup>(6)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Spotlight</td>
        <td align="center">50x50</td>
        <td align="center">100x100</td>
        <td align="center">40x40</td>
        <td align="center">80x80</td>
        <td align="center" style="color:#BBBBBB;">120x120<sup>(5)</sup></td>
    </tr>
    <tr valign="top">
        <td width="200" bgcolor="#F9F9F9" align="right">Параметры</td>
        <td align="center" style="color:#BBBBBB;">29x29<sup>(3)(5)</sup></td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(3)(5)</sup></td>
        <td align="center">-</td>
        <td align="center">-</td>
        <td align="center" style="color:#BBBBBB;">58x58<sup>(5)</sup></td>
    </tr>
</table>

1. _Оба Visual Studio для Mac и Xcode больше не поддерживает задание 1 x изображение для iOS 7._
2. _Изображение iOS 7 1 x не поддерживается при использовании средств каталоги._
3. _iOS 7 и 8 использовать одинаковый объем изображения в качестве iOS 5 и 6._
4. _Использует в качестве значка Spotlight размеры и изображения._
5. _Использует в качестве iPhone того же размера значков._
6. _Поддерживается только с наборами образ ОС каталога._

Дополнительные сведения о значках см. в разделе Apple [размеры изображения и значок](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/MobileHIG/IconMatrix.html#//apple_ref/doc/uid/TP40006556-CH27-SW1) документации.

<a name="managing" />

## <a name="managing-icons-with-asset-catalogs"></a>Управление значки с каталогами активов

Специальные значки `AppIcons` набор изображений, которые могут добавляться в `Assets.xcassets` файл в проекте приложения. Все версии образа, необходимых для поддержки всех разрешениях включаются в _xcasset_ и сгруппированы вместе. Специальный редактор в Visual Studio для Mac разработчик может включить и настроить эти образы графически.

Чтобы использовать каталог средства, выполните следующее:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
2. Прокрутите вниз до **значки приложений** раздела.
3. Из **источника** раскрывающемся списке, убедитесь, **AppIcons** выбран: 

    ![](app-icons-images/migrate01.png "Убедитесь, что выбрана AppIcons")
4. Из **обозревателе решений**, дважды щелкните `Assets.xcassets` файл, чтобы открыть его для редактирования: 

    ![](app-icons-images/asset01.png "В обозревателе решений файл Assets.xcassets")
5. Выберите `AppIcons` из списка активов для отображения `Icon Editor`: 

    ![](app-icons-images/asset02.png "Редактор AppIcons")
6. Либо щелкните кнопкой мыши данный значок типа выберите файл изображения для требуемого типа и размера или изображения из папки и перетащите его на нужный размер.
7. Нажмите кнопку **откройте** кнопку, чтобы включить изображение в проект и задайте его в xcasset.
8. Повторите эти действия для всех необходимых образов.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните **Info.plist** файла в **обозревателе решений**:

    ![](app-icons-images/icon01w.png "Выберите Info.plist")
2. Щелкните **визуальные активы** и выберите **каталога используйте активов** под заголовком **значки приложений**: 

    ![](app-icons-images/icon02w.png "Перейдите на вкладку визуальные активы")
4. Из **обозревателе решений**, разверните **каталога активов** папки: 

    ![](app-icons-images/image009.png "Разверните папку каталога активов")
5. Дважды щелкните **Media** файл, чтобы открыть его в редакторе: 

    ![](app-icons-images/image010.png "Откройте файл мультимедиа в редакторе")
6. В разделе **свойства обозревателя** разработчик может выбрать размеры значков необходимые и различных типов.
7. Щелкните данный тип значка и выберите файл изображения для требуемого типа и размера.
8. Нажмите кнопку **откройте** кнопку, чтобы включить изображение в проект и задайте его в xcasset.
9. Повторите эти действия для всех необходимых образов.

-----

Это предпочтительный метод, включая и управление ими активы изображений, которые будут использоваться для предоставления значков приложения, Spotlight и параметры для приложения.

### <a name="migrating-from-infoplist-to-asset-catalogs"></a>Миграция из Info.plist в каталоги активов

Для существующего приложения Xamarin.iOS с помощью `Info.plist` файла для управления его значки, настоятельно рекомендуется, разработчик переключить использовать `AppIcons` ресурса изображения внутри `Assets.xcassets`.

Выполните следующие действия:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
2. Прокрутите вниз до **значки приложений** раздела.
3. Из **источника** раскрывающегося списка выберите **миграции каталогов активов**: 

    ![](app-icons-images/migrate02.png "Выберите перенести каталоги активов")
4. Существующие значки, определенные в `Info.plist` файла перемещаются в `AppIcons` задать изображение добавлены `Assets.xcassets`: 

     ![](app-icons-images/migrate03.png "Задать изображение AppIcons в Assets.xcassets")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
2. Щелкните на iPhone разделе значков: 

    ![](app-icons-images/image007.png "Редактор значков активное охлаждение iPhone")
3. Прокрутите вниз до **значки** раздела.
4. Из **каталога активов** раскрывающегося списка выберите **каталоги активов используйте**.
5. Существующие значки, определенные в `Info.plist` файла перемещаются в `Images` добавлен набор `Assets.xcassets`.
6. Сохраните изменения в файле `Info.plist`.

-----

<a name="itunes" />

## <a name="itunes-artwork"></a>Иллюстрации iTunes

При использовании метода доставки приложение (либо для корпоративных пользователей для бета-тестирования на реальном устройстве) Ad-Hoc разработчик также должен включать 512 x 512 и 1024 x 1024 образ, который будет использоваться для представления приложения в iTunes.

Чтобы указать иллюстрации iTunes, сделайте следующее:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Дважды щелкните `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.
2. Перейдите к пункту **iTunes иллюстрации** разделе редактора: 

    ![](app-icons-images/itunes01.png "Перейдите к iTunes иллюстрации раздел в редакторе")
3. Все отсутствующие изображения, щелкните его в редакторе, выберите файл изображения для иллюстрации требуемой iTunes диалоговое окно «Открытие файла» и нажмите кнопку **ОК** кнопки.
4. Повторите этот шаг, пока все необходимые заданы образы для приложения.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Дважды щелкните `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования.

2. Щелкните **визуальные активы** и разверните **iTunes иллюстрации**: 

    ![](app-icons-images/itunes01w.png "Редактирование iTunes графических изображений в Visual Studio")
4. Все отсутствующие изображения, щелкните его в редакторе, выберите файл изображения для иллюстрации требуемой iTunes диалоговое окно «Открытие файла» и нажмите кнопку **откройте** кнопки.
5. Повторите этот шаг, пока все необходимые заданы образы для приложения.

-----

## <a name="related-links"></a>Связанные ссылки

- [Работа с изображениями (пример)](https://developer.xamarin.com/samples/WorkingWithImages/)
- [Hello, iPhone](~/ios/get-started/hello-ios/index.md)
- [Пользовательский значок и правила создания образа](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/IconsImages/IconsImages.html))
