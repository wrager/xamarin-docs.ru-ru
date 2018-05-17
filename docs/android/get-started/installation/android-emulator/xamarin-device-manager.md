---
title: Управление эмуляторами Android с помощью диспетчера устройств Xamarin Android
description: Это руководство описывает использование диспетчера устройств Xamarin Android для создания и настройки виртуальных устройств Android (AVD), эмулирующих устройства Android. Эти виртуальные устройства позволяют запустить и протестировать приложение без использования физического устройства.
ms.prod: xamarin
ms.assetid: ECB327F3-FF1C-45CC-9FA6-9C11032BD5EF
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/03/2018
ms.openlocfilehash: 420ffc905659c6fd6245dc8cc3bdae4cb9401a63
ms.sourcegitcommit: e16517edcf471b53b4e347cd3fd82e485923d482
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="xamarin-android-device-manager"></a>Диспетчер устройств Xamarin Android

_Это руководство описывает использование Xamarin Android Device Manager для создания и настройки виртуальных устройств Android (AVD), эмулирующих устройства Android. Эти виртуальные устройства позволяют запускать и тестировать приложение без использования физического устройства._

## <a name="overview"></a>Обзор

После проверки включения аппаратного ускорения (как описано в статье [Аппаратное ускорение](~/android/get-started/installation/android-emulator/hardware-acceleration.md)) с помощью Xamarin Android Device Manager создайте виртуальные устройства, которые будут использоваться для тестирования и отладки приложения.


# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Это руководство описывает, как использовать Xamarin Android Device Manager для Visual Studio для Windows (или [Mac](?tabs=vsmac)):

[![Снимок экрана с диспетчером устройств Xamarin Android на вкладке устройств](xamarin-device-manager-images/win/01-devices-dialog-sml.png)](xamarin-device-manager-images/win/01-devices-dialog.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Это руководство описывает, как использовать Xamarin Android Device Manager для Visual Studio для Mac (или [Windows](?tabs=vswin)):

[![Снимок экрана с диспетчером устройств Xamarin Android на вкладке устройств](xamarin-device-manager-images/mac/01-devices-dialog-sml.png)](xamarin-device-manager-images/mac/01-devices-dialog.png#lightbox)

> [!NOTE]
> Это руководство распространяется только на Visual Studio для Mac.
Xamarin Studio не совместим с диспетчером устройств Xamarin Android.

-----

Диспетчер устройств Xamarin Android предназначен для создания и настройки *виртуальных устройств Android* (AVD), запускаемых в [эмуляторе SDK для Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md).
Каждое AVD является конфигурацией эмулятора, имитирующей физическое устройство Android. Это позволяет запускать и тестировать приложение в различных конфигурациях, имитирующих различные физические устройства Android. Диспетчер устройств Xamarin Android заменяет автономный диспетчер устройств AVD от Google (который был признан нерекомендуемым).

В этом руководстве вы научитесь создавать, дублировать, настраивать и запускать виртуальные устройства с помощью Android Device Manager. Это руководство также описывает, как настроить свойства для каждого виртуального устройства (например, уровень API, ЦП, память и разрешение) для включения или отключения имитируемых датчиков, таких как акселерометр, GPS, датчики ориентации и освещенности, а также настройки типа аппаратного ускорения, используемого этим виртуальным устройством.

## <a name="requirements"></a>Требования

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Чтобы использовать диспетчер устройств Xamarin Android, необходимо следующее:

- Требуется Visual Studio 2017 версии 15.7 или более поздней. Поддерживается Visual Studio Community и более поздние выпуски.

- Xamarin для Visual Studio версии 4.9 или более поздней. Сведения об обновлении Xamarin см. в разделе [Смена канала обновления](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/).

- **Пакет SDK для Android** &ndash; Нужно установить пакет SDK для Android (см. раздел [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md)) и SDK Tools версии 26.0 или более поздней, как описано в следующем разделе. Обязательно установите пакет SDK для Android в следующее расположение (если это еще не сделано): **C:\\Program Files (x86)\\Android\\android-sdk**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

- Visual Studio для Mac 7.5 или более поздней версии.

- **Пакет SDK для Android** &ndash; — нужно установить пакет SDK для Android 8.0 (API 26) или более поздней версии через диспетчер пакетов SDK.

-----

## <a name="launching-the-device-manager"></a>Запуск диспетчера устройств

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Запустите Xamarin Android Device Manager в меню **Сервис**, выбрав **Сервис > Диспетчер эмулятора Android**:

[![Запуск из меню "Сервис"](xamarin-device-manager-images/win/04-tools-menu-sml.png)](xamarin-device-manager-images/win/04-tools-menu.png#lightbox)

Если вы видите следующее диалоговое окно ошибки при запуске, см. инструкции по обходу в разделе [Устранение неполадок](#troubleshooting):

![Ошибка экземпляра SDK для Android](xamarin-device-manager-images/win/32-sdk-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

В Visual Studio для Mac запустите Xamarin Android Device Manager, выбрав **Сервис > Диспетчер эмуляторов**:

[![Запуск из меню "Сервис"](xamarin-device-manager-images/mac/16-tools-menu-sml.png)](xamarin-device-manager-images/mac/16-tools-menu.png#lightbox)

-----

Прежде чем использовать диспетчер устройств Android, нужно установить Android SDK Tools версии 26.0.0 или более поздней. При отсутствии Android SDK Tools версии 26.0.0 или более поздней при запуске появляется следующее диалоговое окно ошибки:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Диалоговое окно ошибки экземпляра SDK для Android](xamarin-device-manager-images/win/02-sdk-instance-error.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![Диалоговое окно ошибки экземпляра SDK для Android](xamarin-device-manager-images/mac/02-sdk-instance-error.png)

-----

Если вы видите это диалоговое окно ошибки, нажмите кнопку **ОК**, чтобы открыть диспетчер пакетов SDK для Android. В диспетчере пакетов SDK для Android откройте вкладку **Сервис** и установите **Android SDK Tools 26.0.2** или более поздней версии, **инструменты платформы SDK для Android 26.0.0** или более поздней версии и **инструменты сборки SDK для Android 26.0.0** или более поздней версии:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Установка Android SDK Tools 26.0](xamarin-device-manager-images/win/03-sdk-tools-sml.png)](xamarin-device-manager-images/win/03-sdk-tools.png#lightbox)

После установки этих пакетов можно закрыть диспетчер пакетов SDK и повторно запустить диспетчер устройств Android.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Установка Android SDK Tools 26.0](xamarin-device-manager-images/mac/03-sdk-tools-sml.png)](xamarin-device-manager-images/mac/03-sdk-tools.png#lightbox)

-----

## <a name="main-screen"></a>Главный экран

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

При первом запуске диспетчер устройств Android выводит экран, где отображаются все настроенные виртуальные устройства. Для каждого устройства указаны параметры **Имя**, **Операционная система** (уровень API Android), **ЦП**, **Память**, а также разрешение экрана:

[![Список установленных устройств и их параметров](xamarin-device-manager-images/win/05-installed-list-sml.png)](xamarin-device-manager-images/win/05-installed-list.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

При первом запуске диспетчер устройств Android выводит экран, где отображаются все настроенные виртуальные устройства. Для каждого устройства указаны параметры **Имя**, **Образ системы** (уровень API Android), **ЦП**, **Память**, а также разрешение экрана:

[![Список установленных устройств и их параметров](xamarin-device-manager-images/mac/05-devices-list-sml.png)](xamarin-device-manager-images/mac/05-devices-list.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Если щелкнуть устройство в списке, справа отображается кнопка **Запустить**. Можно нажать кнопку **Запустить**, чтобы запустить эмулятор с этим виртуальным устройством:

[![Кнопка "Запустить" для образа устройства](xamarin-device-manager-images/win/06-start-button-sml.png)](xamarin-device-manager-images/win/06-start-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Нажмите **Воспроизвести**, чтобы запустить эмулятор с выбранным виртуальным устройством:

[![Кнопка "Запустить" для образа устройства](xamarin-device-manager-images/mac/06-start-button-sml.png)](xamarin-device-manager-images/mac/06-start-button.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

После запуска эмулятора с выбранным виртуальным устройством кнопка **Запустить** изменяется на кнопку **Остановить**, позволяющую остановить эмулятор:

[![Кнопка "Остановить" для работающего устройства](xamarin-device-manager-images/win/07-stop-button-sml.png)](xamarin-device-manager-images/win/07-stop-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

После запуска эмулятора с выбранным виртуальным устройством кнопка **Воспроизвести** изменяется на кнопку **Остановить**, позволяющую остановить эмулятор:

[![Кнопка "Остановить" для работающего устройства](xamarin-device-manager-images/mac/07-stop-button-sml.png)](xamarin-device-manager-images/mac/07-stop-button.png#lightbox)

-----

### <a name="new-device"></a>Создание устройства

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Чтобы создать устройство, нажмите кнопку **Создать** в верхней правой части экрана:

[![Кнопка "Создать" для создания устройства](xamarin-device-manager-images/win/08-new-button-sml.png)](xamarin-device-manager-images/win/08-new-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Чтобы создать устройство, нажмите кнопку **Создать устройство** в верхней правой части экрана:

[![Кнопка "Создать" для создания устройства](xamarin-device-manager-images/mac/08-new-button-sml.png)](xamarin-device-manager-images/mac/08-new-button.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

При нажатии кнопки **Создать** открывается экран **Новое устройство**:

[![Экран "Новое устройство" диспетчера устройств](xamarin-device-manager-images/win/09-new-device-editor-sml.png)](xamarin-device-manager-images/win/09-new-device-editor.png#lightbox)

Чтобы настроить новое устройство на экране **Новое устройство**, выполните следующие действия:

1. Выберите физическое устройство для эмуляции, щелкнув раскрывающееся меню **Устройство**:

    [![Раскрывающееся меню "Устройство"](xamarin-device-manager-images/win/10-device-menu-sml.png)](xamarin-device-manager-images/win/10-device-menu.png#lightbox)

2. Выберите образ системы для использования с этим виртуальным устройством, щелкнув раскрывающееся меню **Образ системы**. В разделе **Установленные** этого меню перечислены установленные образы системы. В разделе **Загрузки** перечислены образы системы, которые сейчас недоступны на компьютере разработчика, но могут быть установлены автоматически:

    [![Раскрывающееся меню "Образ системы"](xamarin-device-manager-images/win/11-system-image-menu-sml.png)](xamarin-device-manager-images/win/11-system-image-menu.png#lightbox)

3. Задайте новое имя для устройства. В следующем примере новое устройство называется **Nexus 5 API 25**:

    [![Именование нового устройства](xamarin-device-manager-images/win/12-device-name-sml.png)](xamarin-device-manager-images/win/12-device-name.png#lightbox)

4. Измените нужные свойства. Сведения о том, как это сделать, см. в разделе [Свойства профиля](#properties) ниже.

5. Добавьте любые дополнительные свойства, которые нужно задать явно. На экране **Новое устройство** перечислены только часто изменяемые свойства, но можно щелкнуть раскрывающееся меню **Добавление свойства** в нижнем левом углу, чтобы добавить дополнительные свойства. В следующем примере добавляется свойство `hw.lcd.backlight`:

    [![Раскрывающееся меню "Добавление свойства"](xamarin-device-manager-images/win/13-add-property-menu-sml.png)](xamarin-device-manager-images/win/13-add-property-menu.png#lightbox)

6. Нажмите кнопку **Создать** в нижнем правом углу, чтобы создать устройство:

    ![Кнопка "Создать"](xamarin-device-manager-images/win/14-create-button.png)

7. Может появиться экран **Принятие условий лицензионного соглашения**. Если вы согласны с условиями лицензии, щелкните **Принять**:

    ![Экран "Принятие условий лицензионного соглашения"](xamarin-device-manager-images/win/15-license-acceptance.png)

8. Пока устройство создается, диспетчер устройств Android добавляет его в список установленных виртуальных устройств с индикатором хода выполнения **Создание**:

    [![Индикатор хода выполнения "Создание"](xamarin-device-manager-images/win/16-creating-the-device-sml.png)](xamarin-device-manager-images/win/16-creating-the-device.png#lightbox)

9. После завершения создания новое устройство отображается в списке установленных виртуальных устройств с кнопкой **Запустить**, готовое к запуску:

   [![Только что созданное устройство, готовое к запуску](xamarin-device-manager-images/win/17-created-device-sml.png)](xamarin-device-manager-images/win/17-created-device.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

При нажатии кнопки **Создать устройство** открывается экран **Новое устройство**:

[![Экран "Новое устройство" диспетчера устройств](xamarin-device-manager-images/mac/09-new-device-editor-sml.png)](xamarin-device-manager-images/mac/09-new-device-editor.png#lightbox)

Чтобы настроить новое устройство на экране **Новое устройство**, выполните следующие действия:

1. Выберите физическое устройство для эмуляции, щелкнув раскрывающееся меню **Устройство**:

    [![Раскрывающееся меню "Устройство"](xamarin-device-manager-images/mac/10-device-menu-sml.png)](xamarin-device-manager-images/mac/10-device-menu.png#lightbox)

2. Выберите образ системы для использования с этим виртуальным устройством, щелкнув раскрывающееся меню **Образ системы**. В разделе **Установленные** этого меню перечислены установленные образы системы. В разделе **Загрузки** (если он отображается) перечислены образы системы, которые сейчас недоступны на компьютере разработчика, но могут быть установлены автоматически:

    [![Раскрывающееся меню "Образ системы"](xamarin-device-manager-images/mac/11-system-image-menu-sml.png)](xamarin-device-manager-images/mac/11-system-image-menu.png#lightbox)

3. Задайте новое имя для устройства. В следующем примере новое устройство называется **Nexus 5X API 25**:

    [![Именование нового устройства](xamarin-device-manager-images/mac/12-device-name-sml.png)](xamarin-device-manager-images/mac/12-device-name.png#lightbox)

4. Измените нужные свойства. Сведения о том, как это сделать, см. в разделе [Свойства профиля](#properties) ниже.

5. Добавьте любые дополнительные свойства, которые нужно задать явно. На экране **Новое устройство** перечислены только часто изменяемые свойства, но можно щелкнуть раскрывающееся меню **Добавление свойства** в нижнем левом углу, чтобы добавить дополнительные свойства:

    [![Раскрывающееся меню "Добавление свойства"](xamarin-device-manager-images/mac/13-add-property-menu-sml.png)](xamarin-device-manager-images/mac/13-add-property-menu.png#lightbox)

6. Можно также щелкнуть элемент **Настраиваемый**, чтобы определить новое свойство для устройства:

    ![Кнопка "Создать"](xamarin-device-manager-images/mac/14-custom-button.png)

7. Нажмите кнопку **Создать** в нижнем правом углу, чтобы создать устройство:

    ![Кнопка "Создать"](xamarin-device-manager-images/mac/15-create-button.png)

8. Может появиться экран **Принятие условий лицензионного соглашения**. Если вы согласны с условиями лицензии, щелкните **Принять**.

9. Пока устройство создается, диспетчер устройств Android добавляет его в список устройств с индикатором хода выполнения **Создание**:

    [![Индикатор хода выполнения "Создание"](xamarin-device-manager-images/mac/17-creating-the-device-sml.png)](xamarin-device-manager-images/mac/17-creating-the-device.png#lightbox)

10. После завершения создания новое устройство отображается в списке устройств с кнопкой **Воспроизвести**, готовое к запуску:

   [![Только что созданное устройство, готовое к запуску](xamarin-device-manager-images/mac/18-created-device-sml.png)](xamarin-device-manager-images/mac/18-created-device.png#lightbox)

-----


<a name="device-edit" />


### <a name="edit-device"></a>Изменение устройства

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Чтобы изменить существующее виртуальное устройство, выберите его и нажмите кнопку **Изменить** в правом верхнем углу:

[![Кнопка "Изменить" для изменения нового устройства](xamarin-device-manager-images/win/19-edit-button-sml.png)](xamarin-device-manager-images/win/19-edit-button.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Для изменения существующего виртуального устройства щелкните раскрывающееся меню **Дополнительные параметры** (значок шестеренки) и выберите **Изменить**:
 
[![Пункт меню "Изменить" для изменения нового устройства](xamarin-device-manager-images/mac/19-edit-button-sml.png)](xamarin-device-manager-images/mac/19-edit-button.png#lightbox)
 
-----

При щелчке элемента **Изменить** запускается редактор устройств для выбранного виртуального устройства:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Окно редактора устройств](xamarin-device-manager-images/win/20-device-editor-sml.png)](xamarin-device-manager-images/win/20-device-editor.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
 
[![Окно редактора устройств](xamarin-device-manager-images/mac/20-device-editor-sml.png)](xamarin-device-manager-images/mac/20-device-editor.png#lightbox)
 
-----

На экране **Device Editor** (Редактор устройств) в первом столбце перечислены свойства виртуального устройства, а во втором — соответствующие значения для них. При выборе свойства справа отображается его подробное описание.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Например, на следующем снимке экрана свойство `hw.lcd.density` изменяется с **420** на **240**:

[![Пример изменения устройства](xamarin-device-manager-images/win/21-device-editing-sml.png)](xamarin-device-manager-images/win/21-device-editing.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Например, на следующем снимке экрана свойство `hw.lcd.density` изменяется с **320** на **240**, а свойство `hw.ramSize` изменяется на **768**:
 
[![Пример изменения устройства](xamarin-device-manager-images/mac/21-device-editing-sml.png)](xamarin-device-manager-images/mac/21-device-editing.png#lightbox)
 
-----

После внесения необходимых изменений в конфигурацию нажмите кнопку **Сохранить**.
Дополнительные сведения об изменении свойств виртуального устройства см. в разделе [Свойства профиля](#properties) ниже.


 
### <a name="additional-options"></a>Дополнительные параметры

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В меню &hellip; в правом верхнем углу доступны дополнительные параметры для работы с устройствами:

[![Расположение меню с дополнительными параметрами](xamarin-device-manager-images/win/22-overflow-menu-sml.png)](xamarin-device-manager-images/win/22-overflow-menu.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Дополнительные параметры для работы с устройством доступны в раскрывающемся меню, расположенном слева от кнопки **Воспроизвести**:

[![Расположение меню с дополнительными параметрами](xamarin-device-manager-images/mac/22-overflow-menu-sml.png)](xamarin-device-manager-images/mac/22-overflow-menu.png#lightbox)

-----

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Меню с дополнительными параметрами содержит следующие элементы:

-   **Duplicate and Edit** (Дублировать и править) &ndash; дублирует выбранное устройство и открывает экран **Новое устройство** с другим уникальным именем. Например, если выбрать **VisualStudio_android-23_x86_phone** и щелкнуть **Duplicate and Edit** (Дублировать и править), к имени добавляется счетчик:

    [![Экран "Duplicate and Edit" (Дублировать и править)](xamarin-device-manager-images/win/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/win/23-dupe-and-edit.png#lightbox)

-   **Отобразить в проводнике** &ndash; открывает окно проводника в папке с файлами для виртуального устройства. Например, если выбрать **Nexus 5X API 25** и щелкнуть **Отобразить в проводнике**, открывается окно, похожее на следующее:

    [![Результаты выбора параметра "Отобразить в проводнике"](xamarin-device-manager-images/win/24-reveal-in-explorer-sml.png)](xamarin-device-manager-images/win/24-reveal-in-explorer.png#lightbox)

-   **Сброс параметров** &ndash; сбрасывает выбранное устройство в параметры по умолчанию, удаляя любые изменения, внесенные пользователем во внутреннее состояние устройства во время его работы. Эта операция не затрагивает изменения, внесенные на виртуальном устройстве во время его создания и редактирования. Отображается диалоговое окно с напоминанием, что этот сброс невозможно отменить. Нажмите кнопку **Wipe user data** (Очистить пользовательские данные), чтобы подтвердить сброс.

-   **Удалить** &ndash; окончательно удаляет выбранное виртуальное устройство.
    Отображается диалоговое окно с напоминанием, что такое удаление невозможно отменить. Щелкните **Удалить**, если уверены, что устройство нужно удалить.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Меню с дополнительными параметрами содержит следующие элементы:

-   **Изменить** &ndash; открывает выбранное устройство в редакторе устройств, как описано выше в разделе [Изменение устройства](#device-edit).

-   **Duplicate and Edit** (Дублировать и править) &ndash; дублирует выбранное устройство и открывает экран **Новое устройство** с другим уникальным именем.
    Например, если выбрать **Nexus 5X API 25** и щелкнуть **Duplicate and Edit** (Дублировать и править), к имени добавляется счетчик:

    [![Экран "Duplicate and Edit" (Дублировать и править)](xamarin-device-manager-images/mac/23-dupe-and-edit-sml.png)](xamarin-device-manager-images/mac/23-dupe-and-edit.png#lightbox)

-   **Отобразить в Finder** &ndash; открывает окно macOS Finder в папке с файлами для виртуального устройства. Например, если выбрать **Nexus 5X API 25** и щелкнуть **Отобразить в Finder**, открывается окно, похожее на следующее:

    [![Результаты выбора параметра "Отобразить в проводнике"](xamarin-device-manager-images/mac/24-reveal-in-finder-sml.png)](xamarin-device-manager-images/mac/24-reveal-in-finder.png#lightbox)

-   **Сброс параметров** &ndash; сбрасывает выбранное устройство в параметры по умолчанию, удаляя любые изменения, внесенные пользователем во внутреннее состояние устройства во время его работы. Эта операция не затрагивает изменения, внесенные на виртуальном устройстве во время его создания и редактирования. Отображается диалоговое окно с напоминанием, что этот сброс невозможно отменить. Нажмите кнопку **Wipe user data** (Очистить пользовательские данные), чтобы подтвердить сброс.

-   **Удалить** &ndash; окончательно удаляет выбранное виртуальное устройство.
    Отображается диалоговое окно с напоминанием, что такое удаление невозможно отменить. Щелкните **Удалить**, если уверены, что устройство нужно удалить.

-----

<a name="properties" />
 

## <a name="profile-properties"></a>Свойства профиля

На экранах **Создание устройства** и **Изменение устройства** в первом столбце перечислены свойства виртуального устройства, а во втором — соответствующие значения для них. При выборе свойства справа отображается его подробное описание. Вы можете изменить *свойства профиля оборудования* и *свойства AVD*.
Свойства профиля оборудования (например, `hw.ramSize` и `hw.accelerometer`) описывают физические характеристики эмулируемого устройства. К таким характеристикам относится размер экрана, объем доступной оперативной памяти, состояние работы акселерометра. Свойства AVD описывают работу AVD. Например, в свойствах AVD можно указать, как AVD использует видеоадаптер вашего компьютера разработчика для отрисовки.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Ниже описано, как можно изменить свойства:

-   Чтобы изменить логическое свойство, щелкните флажок справа от него:

    ![Изменение логического свойства](xamarin-device-manager-images/win/25-boolean-value.png)

-   Чтобы изменить свойство *enum* (перечисление), щелкните стрелку вниз справа от свойства и выберите новое значение.

    ![Изменение свойства перечисления](xamarin-device-manager-images/win/27-enum-value.png)

-   Чтобы изменить строковое или целочисленное свойство, дважды щелкните текущую строку или целое число в столбце значения и введите новое значение.

    ![Изменение целочисленного свойства](xamarin-device-manager-images/win/26-integer-value.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Ниже описано, как можно изменить свойства:

-   Чтобы изменить логическое свойство, щелкните флажок справа от него:

    ![Изменение логического свойства](xamarin-device-manager-images/mac/25-boolean-value.png)

-   Чтобы изменить свойство *enum* (перечисление), щелкните раскрывающееся меню справа от свойства и выберите новое значение.

    ![Изменение свойства перечисления](xamarin-device-manager-images/mac/27-enum-value.png)

-   Чтобы изменить строковое или целочисленное свойство, дважды щелкните текущую строку или целое число в столбце значения и введите новое значение.

    ![Изменение целочисленного свойства](xamarin-device-manager-images/mac/26-integer-value.png)


-----

Следующая таблица содержит подробное описание свойств, указанных на экранах **Новое устройство** и **Device Editor** (Редактор устройств):

[!include[](~/android/includes/emulator-properties.md)]

Дополнительные сведения об этих свойствах см. в разделе [Свойства профиля оборудования](https://developer.android.com/studio/run/managing-avds.html#hpproperties).


<a name="troubleshooting" />

## <a name="troubleshooting"></a>Устранение неполадок

Ниже описаны распространенные проблемы диспетчера устройств Xamarin Android и способы их устранения:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

### <a name="android-sdk-in-non-standard-location"></a>Пакет SDK для Android в нестандартном расположении

Как правило, пакет SDK для Android устанавливается в следующем расположении:

**C:\\Program Files (x86)\\Android\\android-sdk**

Если пакет SDK не установлен в этом расположении, при запуске может возникнуть следующая ошибка:

![Ошибка экземпляра SDK для Android](xamarin-device-manager-images/win/32-sdk-error.png)

Чтобы избежать этой проблемы, выполните следующие действия:

1. На рабочем столе Windows перейдите в **C:\\Users\\*имя_пользователя*\\AppData\\Roaming\\XamarinDeviceManager**:

    ![Расположение файла журнала для диспетчера устройств Xamarin](xamarin-device-manager-images/win/33-log-files.png)

2. Дважды щелкните, чтобы открыть один из файлов журнала, и найдите **Путь к файлу конфигурации**. Пример:

    [![Путь к файлу конфигурации в файле журнала](xamarin-device-manager-images/win/34-config-file-path-sml.png)](xamarin-device-manager-images/win/34-config-file-path.png#lightbox)

3. Перейдите в это расположение и дважды щелкните **user.config**, чтобы открыть его. 

4. В **user.config** найдите элемент **&lt;UserSettings&gt;** и добавьте к нему атрибут **AndroidSdkPath**. Задайте для этого атрибута путь, по которому установлен пакет SDK для Android на вашем компьютере, и сохраните файл. Например, **&lt;UserSettings&gt;** будет выглядеть следующим образом, если пакет SDK для Android был установлен в **C:\\Programs\\Android\\SDK**:
        
    ```xml
    <UserSettings SdkLibLastWriteTimeUtcTicks="636409365200000000" AndroidSdkPath="C:\Programs\Android\SDK" />
    ```

Внеся это изменение в **user.config**, можно станет запустить диспетчер устройств Android Xamarin.

### <a name="snapshot-disables-wifi-on-android-oreo"></a>При создании моментального снимка в Android Oreo отключается Wi-Fi

Если для Android Oreo настроено виртуальное устройство AVD с имитацией доступа по Wi-Fi, перезапуск AVD после создания моментального снимка может привести к отключению доступа по Wi-Fi.

Действия по решению этой проблемы:

1. В диспетчере устройств Xamarin выберите AVD.

2. В меню дополнительных параметров выберите пункт **Отобразить в проводнике**.

3. Последовательно выберите **snapshots (моментальные снимки) > default_boot**.

4. Удалите файл **snapshot.pb**:

    [![Расположение файла snapshot.pb](xamarin-device-manager-images/win/36-delete-snapshot-sml.png)](xamarin-device-manager-images/win/36-delete-snapshot.png#lightbox)

5. Перезапустите AVD. 

После внесения этих изменений AVD перезапустится в состоянии, которое позволяет возобновить работу Wi-Fi.


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

### <a name="snapshot-disables-wifi-on-android-oreo"></a>При создании моментального снимка в Android Oreo отключается Wi-Fi

Если для Android Oreo настроено виртуальное устройство AVD с имитацией доступа по Wi-Fi, перезапуск AVD после создания моментального снимка может привести к отключению доступа по Wi-Fi.

Действия по решению этой проблемы:

1. В диспетчере устройств Xamarin выберите AVD.

2. В меню дополнительных параметров выберите пункт **Отобразить в средстве поиска**.

3. Последовательно выберите **snapshots (моментальные снимки) > default_boot**.

4. Удалите файл **snapshot.pb**:

    [![Расположение файла snapshot.pb](xamarin-device-manager-images/mac/36-delete-snapshot-sml.png)](xamarin-device-manager-images/mac/36-delete-snapshot.png#lightbox)

5. Перезапустите AVD. 

После внесения этих изменений AVD перезапустится в состоянии, которое позволяет возобновить работу Wi-Fi.

-----


### <a name="generating-a-bug-report"></a>Создание отчета об ошибках

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Если вы обнаружили проблему с диспетчером устройств Xamarin Android, которую не удается разрешить с помощью приведенных выше советов, отправьте отчет об ошибках, щелкнув правой кнопкой мыши строку заголовка и выбрав пункт **Generate Bug Report** (Создать отчет об ошибках):

![Расположение пункта меню для заполнения отчета об ошибках](xamarin-device-manager-images/win/35-bug-report.png)


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Если вы обнаружили проблему с диспетчером устройств Xamarin Android, которую не удается решить с помощью приведенных выше советов, отправьте отчет об ошибках. Для этого последовательно выберите **Help > Generate Bug Report (Справка > Создать отчет об ошибках)**:

![Расположение пункта меню для заполнения отчета об ошибках](xamarin-device-manager-images/mac/35-bug-report.png)

-----

 
 
## <a name="summary"></a>Сводка

Это руководство содержит общие сведения о диспетчере устройств Xamarin Android, доступном в Visual Studio для Mac и Xamarin для Visual Studio. Оно описывает важные функции, такие как запуск и остановка эмулятора Android, выбор виртуального устройства Android (AVD) для запуска, создание и изменение виртуальных устройств. Оно также описывает, как изменить свойства профиля оборудования для дальнейшей настройки.


## <a name="related-links"></a>Связанные ссылки

- [Изменения в инструментарии пакета SDK для Android](~/android/troubleshooting/sdk-cli-tooling-changes.md)
- [Отладка с помощью эмулятора SDK для Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)
- [Заметки о выпуске пакета SDK Tools (Google)](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
