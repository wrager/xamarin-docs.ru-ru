---
title: "Работа с использованием списков свойств"
description: "В этом документе рассматривается Visual Studio для Mac в редактор списка (.plist) графический и расширенные свойства для работы с Info.plist и Entitlements.plist. Он иллюстрирует параметр значки и запуска изображений для приложений iOS в Visual Studio для Mac."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5E687043-0443-377C-9A12-9C5A05958646
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 778e70f6817b71e5910aa85425d46261dfe9c803
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-property-lists"></a>Работа с использованием списков свойств

_В этом документе рассматривается Visual Studio для Mac в редактор списка (.plist) графический и расширенные свойства для работы с Info.plist и Entitlements.plist. Он иллюстрирует параметр значки и запуска изображений для приложений iOS в Visual Studio для Mac._

Visual Studio для Mac включает .plist графический редактор, позволяющая редактировать свойства приложения и возможности проще. Visual Studio для Mac имеет два .plists - `Info.plist` для редактирования свойств приложения и значки, и `Entitlements.plist` для управления возможностями приложения. В этом руководстве представлены Info.plists и общие сведения о работе с ними в Visual Studio для Mac. Сведения о Entitlements.plist см. в разделе [работа с данными](~/ios/deploy-test/provisioning/entitlements.md) руководства.

## <a name="infoplist"></a>Info.plist

Список свойств сведения ( `Info.plist`) — iOS требуется файл, который содержит сведения о конфигурации вашего приложения в системе. Visual Studio для Mac пользовательских `Info.plist` функций редактора, три панели управляются вкладок в нижней левой части окна редактора:

 [![](property-lists-images/tabs.png "Вкладки редактора Info.plist в нижней левой части окна редактора")](property-lists-images/tabs.png#lightbox)

Каждой панели на различных свойств элементов управления, как показано ниже:

-  **Панель приложения** -графический интерфейс для настройки общих свойств приложения, а также значки и запуска образов; укажите интеграции карты и backgrounding режимы.
-  **Дополнительные панели** -«Дополнительно» служит для указания типов поддерживаемых документов, UTIs и типы URL-адрес.
-  **Панель источника** -панели «Источник» управляет менее общие свойства, а также настраиваемые свойства для приложения.


Следующих трех разделах изучить функции каждой панели более подробно.

## <a name="application-panel"></a>Панель приложений

Графический интерфейс для редактирования общих функций Visual Studio для Mac `Info.plist` записи для приложения:

1.  Свойства приложения
1.  Типы поддерживаемых устройств
1.  Поддержка ориентации для каждого типа устройств
1.  Строки, стиль и цвет состояния
1.  Значки и экраны при запуске
1.  Карты и фоновые режимы


Они описаны более подробно в следующих разделах.

 <a name="iOS_Application_Target" />


### <a name="ios-application-target"></a>iOS целевого приложения

Этот раздел содержит важные сведения, описывающие ваше приложение.
**Идентификатор** хранятся здесь должен совпадать с идентификатором пакета, который вводится в iTunes Connect (для приложения для магазина), а также в списке инициализации идентификаторы приложений портала iOS и разработку и распространения сертификатов.

 [![](property-lists-images/image24.png "iOS целевого приложения")](property-lists-images/image24.png#lightbox)

### <a name="device-deployment"></a>Развертывание устройств

 [![](property-lists-images/deployment.png "Развертывание устройств")](property-lists-images/deployment.png#lightbox)

Устройство **развертывания** сведения о разделах отображаются выборочно, в зависимости от выбора в **устройств** раскрывающийся список в **целевого приложения** выше. **Главного интерфейса** имеет значение раскрывающегося списка **MainStoryboard** в приложения на основе раскадровки. Если пользовательский интерфейс полностью записывается в коде, то это может быть пустым.

### <a name="supported-device-orientations"></a>Поддерживаемое устройство ориентации

 **Поддерживаемые ориентации устройства** определяет, как приложение реагирует на поворот устройства. Очень часто для iPhone и iPad приложений для поддержки только **Книжная**, или все данные, но **сверху вниз**. Обычно все приложения iPad, за исключением игры должен поддерживать все ориентации.

### <a name="status-bar-styles"></a>Стили строки состояния

**Стили полосы состояния** раздел — это графический интерфейс для редактирования приложения `UIStatusBarStyle`:

 [![](property-lists-images/status.png "Стили строки состояния")](property-lists-images/status.png#lightbox)

 <a name="Icons" />


### <a name="icons-launch-images-and-itunes-artwork"></a>Значки, изображения, запуска и iTunes иллюстрации

Сведения об использовании значки, изображения и изображения в файле Info.plist можно найти в [работа с образами](~/ios/app-fundamentals/images-icons/index.md) руководства.




### <a name="maps-integration-and-background-modes"></a>Интеграция карты и фоновые режимы

`Info.plist` Содержит специальные разделы для указания интеграции карты и backgrounding режимы. Выбор параметров, которые требуется поддерживать добавит необходимые свойства приложение за вас.

 [![](property-lists-images/maps.png "Интеграция карты")](property-lists-images/maps.png#lightbox)

Дополнительные сведения о работе с картами, см. на странице Xamarin [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md) руководства.

 [![](property-lists-images/bging.png "Фоновые режимы")](property-lists-images/bging.png#lightbox)

Дополнительные сведения о фоновых режимов см. на странице Xamarin [Backgrounding в iOS](~/ios/app-fundamentals/backgrounding/introduction-to-backgrounding-in-ios.md) руководства.

## <a name="advanced-panel"></a>«Дополнительно»

«Дополнительно» для управления типов документов и URL-схем, которые поддерживает приложение.

 [![](property-lists-images/image34.png "«Дополнительно»")](property-lists-images/image34.png#lightbox)

 <a name="Document_Types" />


## <a name="document-types"></a>Типы документов

Для приложений, которые поддерживает открытие определенных типов файлов, предоставляет iOS `CFBundleDocumentTypes` ключа. Если мы хотим нашего приложения для поддержки определенных зарегистрированных типов файлов - например, PDF - мы бы добавили PDF значение для ключа. В этом разделе предоставляет удобный способ для ввода данных, которая будет храниться в `CFBundleDocumentTypes` ключа в `Info.plist` файла.

Обратитесь к документации на [регистрация файл типы Your App поддерживает](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html) подробные сведения о настройке этих значений.

## <a name="utis"></a>UTIs

Иногда приложения должен поддерживать Открытие пользовательского типа файлов. Например, нужно открыть файлы изображений с расширением пользовательских *.xam*. Для указания пользовательского типа файлов, мы создадим пользовательской UTI - универсальный идентификатор типа - с использованием `UIExportedTypeDeclarations` ключа. На снимке экрана ниже показано создание пользовательского UTI для расширения .xam:

 [![](property-lists-images/uti.png "Редактор UTIs")](property-lists-images/uti.png#lightbox)

Как только что экспортированный тип UTIs указать пользовательские UTIs определенному приложению, *импортированный тип UTIs* ( `UIImportedTypeDeclarations` ключ) укажите пользовательские типы поддерживаются, но не принадлежат приложения.

Дополнительные сведения об использовании пользовательских UTIs посвящены Apple [регистрация файл типы Your App поддерживает](https://developer.apple.com/library/ios/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_declare/understand_utis_declare.html#//apple_ref/doc/uid/TP40001319-CH204-SW1) руководства.

## <a name="custom-urls"></a>Пользовательский URL-адреса

Имя схемы URL-адрес (также называемые протокол) — первая часть URL-адрес. Например `http://` и `https://` приведены распространенные схемы URL-адрес. У вас есть возможность создания пользовательской схемы URL-адрес для вашего приложения. Пользовательский URL-схем используются для взаимодействия и передачи данных и обратно с другими приложениями. Следующем снимке экрана показано создание новой пользовательской схемы URL-адрес вызывается `monkeys://`:

 [![](property-lists-images/url.png "Пользовательский URL-адреса")](property-lists-images/url.png#lightbox)



Дополнительные сведения о реализации пользовательской схемы URL-адресов см. Apple [реализации пользовательские схемы URL-адрес раздела этого руководства](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)

## <a name="source-panel"></a>«Источник»

**Источника** вкладке `Info.plist` файл позволяет пользовательские значения, которое необходимо добавить или изменить. Visual Studio для Mac приведен список наиболее общих свойств.

 [![](property-lists-images/image31.png "Добавление нового свойства из раскрывающегося списка")](property-lists-images/image31.png#lightbox)

Для известных свойств Visual Studio для Mac будет список допустимых значений, как показано на следующем снимке экрана:

 [![](property-lists-images/image32.png "Выберите значение из списка известных значений")](property-lists-images/image32.png#lightbox)

Visual Studio для Mac также определяет тип свойства, как показано:

 [![](property-lists-images/image33.png "Доступные типы свойств")](property-lists-images/image33.png#lightbox)

Просмотрите Apple [связанные ресурсы приложения](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html) содержатся ссылки на дополнительные сведения о дополнительных свойств.

 <a name="Entitlements" />

## <a name="summary"></a>Сводка

В этой статье демонстрируется использование .plist графический и расширенные редакторы для редактирования стандартных конфигураций приложений также указать значки и запустите изображения. Он также появился `Entitlements.plist` для добавления и управления ими возможностей приложения.


## <a name="related-links"></a>Связанные ссылки

- [ИНТЕГРИРОВАННАЯ СРЕДА РАЗРАБОТКИ](https://developer.xamarin.com/recipes/cross-platform/ide)
- [Ресурсы, связанные с приложения](http://developer.apple.com/library/ios/#DOCUMENTATION/iPhone/Conceptual/iPhoneOSProgrammingGuide/App-RelatedResources/App-RelatedResources.html)
- [Файл регистрации типов поддерживается данным приложения](http://developer.apple.com/library/ios/#documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Articles/RegisteringtheFileTypesYourAppSupports.html)
- [Реализация пользовательских URL-схем](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/AdvancedAppTricks/AdvancedAppTricks.html)
- [О каталогах активов](https://developer.apple.com/library/ioshttps://developer.xamarin.com/recipes/xcode_help-image_catalog-1.0/Recipe.html)
