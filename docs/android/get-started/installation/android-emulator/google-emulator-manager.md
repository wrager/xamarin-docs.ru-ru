---
title: "Диспетчер эмуляторов Google"
description: "Создание виртуальных устройств Android и управление ими с помощью диспетчера эмуляторов Google"
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C0BBEC0-C84A-4558-B905-4EF81FCD62F9
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 12/22/2017
ms.openlocfilehash: f275ff6c7d3e6eeec5eb3878cc39633d70238f66
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="google-emulator-manager"></a>Диспетчер эмуляторов Google

Следующим шагом после проверки включения аппаратного ускорения (как описано в статье [Аппаратное ускорение эмулятора Android](~/android/get-started/installation/android-emulator/hardware-acceleration.md)) является создание виртуальных устройств, которые будут использоваться для тестирования и отладки приложения. Создавать виртуальные устройства для эмулятора SDK для Android можно с помощью устаревшего диспетчера эмуляторов Google (также известного как *диспетчер виртуальных устройств Android (AVD)*).

> [!NOTE]
> **Примечание.** Если вы работаете с Android 8.0 Oreo, для создания и настройки виртуальных устройств необходимо использовать [диспетчер устройств Android Xamarin](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).

<a name="sysimg" />

## <a name="installing-system-images"></a>Установка образов системы

В зависимости от целевого уровня API Android необходимо скачать и установить соответствующие уровню API образы системы, используемые эмулятором SDK для Android. Для каждого уровня API Android существует набор образов системы **x86**, который нужно скачать и установить для создания виртуальных устройств.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Чтобы установить необходимые образы системы, запустите диспетчер пакетов SDK для Android (**Сервис > Android > Диспетчер пакетов SDK для Android**) и найдите нужные уровни API. Для каждого уровня API установите флажок рядом со следующими образами систем:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Чтобы установить необходимые образы системы, запустите диспетчер пакетов SDK для Android (**Сервис > Диспетчер пакетов SDK**) и найдите нужные уровни API. Для каждого уровня API установите флажок рядом со следующими образами систем:

-----

-   **Образ системы Intel x86 Atom**
-   **Образ системы Google API Intel x86 Atom**

Последний образ системы добавляет API-интерфейсы Google (например, API Google Maps) в виртуальное устройство. 

На следующем снимке экрана показано, что будут установлены образы **Intel x86 Atom** для создания виртуальных устройств под управлением Android 6.0:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Выбор образов системы Android 6.0 x86 для эмулятора Android](google-emulator-manager-images/win/03-select-x86-images-sml.png)](google-emulator-manager-images/win/03-select-x86-images.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Выбор образов системы Android 6.0 x86 для эмулятора Android](google-emulator-manager-images/mac/02-select-x86-images-sml.png)](google-emulator-manager-images/mac/02-select-x86-images.png)

-----

При разработке 64-разрядных приложений установите следующие образы системы:

-   **Образ системы Intel x86 Atom_64**
-   **Образ системы Google API Intel x86 Atom_64**

Эти 64-разрядные системные образы можно использовать для запуска 32-разрядных приложений. Однако 32-разрядный **образ системы Intel x86 Atom** выполняется немного быстрее в эмуляторе SDK для Android.

При разработке приложений для Android Wear установите следующие образы системы:

-   **Образ системы Android Wear Intel x86 Atom**
-   **Образ системы Google API Intel x86 Atom**

После установки этих образов системы можно создать виртуальные устройства Android на базе архитектуры **x86**, выбрав соответствующий уровень API и варианты ЦП/ABI во время настройки виртуального устройства (описывается далее).


<a name="virtualdevice" />

## <a name="configuring-virtual-devices"></a>Настройка виртуальных устройств

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Виртуальные устройства настраиваются с помощью **диспетчера эмуляторов Android** (также называемого _диспетчером виртуальных устройств Android_ или _диспетчером AVD_). Чтобы запустить диспетчер эмуляторов Android из Visual Studio, щелкните значок **диспетчера эмуляторов Android** на панели инструментов:

[ ![Расположение значка AVD](google-emulator-manager-images/win/04-avd-icon-sml.png)](google-emulator-manager-images/win/04-avd-icon.png)

Диспетчер эмуляторов Android можно также запустить из строки меню, последовательно выбрав **Сервис > Android > Диспетчер эмуляторов Android**:

[![Расположение элементов меню диспетчера эмуляторов Android](google-emulator-manager-images/win/05-avd-manager-menu-item-sml.png)](google-emulator-manager-images/win/05-avd-manager-menu-item.png)

В диалоговом окне **Диспетчер виртуальных устройств Android (AVD)** отображается список существующих виртуальных устройств Android:

[![Диспетчер виртуальных устройств Android](google-emulator-manager-images/win/06-virtual-device-manager-sml.png)](google-emulator-manager-images/win/06-virtual-device-manager.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Виртуальные устройства настраиваются с помощью **диспетчера эмуляторов Android** (также называемого _диспетчером виртуальных устройств Android_ или _диспетчером AVD_). 

Диспетчер эмуляторов Android можно запустить из строки меню, последовательно выбрав **Сервис > Диспетчер эмуляторов Google**:

[![Расположение элементов меню диспетчера эмуляторов Android](google-emulator-manager-images/mac/03-avd-manager-menu-item-sml.png)](google-emulator-manager-images/mac/03-avd-manager-menu-item.png)

В диалоговом окне **Диспетчер виртуальных устройств Android (AVD)** отображается список существующих виртуальных устройств Android:

[![Диспетчер виртуальных устройств Android](google-emulator-manager-images/mac/05-virtual-device-manager-sml.png)](google-emulator-manager-images/mac/05-virtual-device-manager.png)

-----

Можно создать образы виртуальных устройств с разными характеристиками устройств и уровнями API. Сведения о создании пользовательских определений устройств и виртуальных устройств приводятся в следующем разделе.

<a name="custom-def" />

### <a name="creating-a-custom-device-definition"></a>Создание пользовательского определения устройства

Чтобы создать пользовательское определение устройства, нажмите кнопку **Создать...** в **диспетчере виртуальных устройств Android (AVD)**. Откроется диалоговое окно **Создание нового виртуального устройства Android (AVD)**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Пользовательское определение устройства на базе Nexus 6](google-emulator-manager-images/win/07-custom-device-sml.png)](google-emulator-manager-images/win/07-custom-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Пользовательское определение устройства на базе Nexus 6](google-emulator-manager-images/mac/06-custom-device-sml.png)](google-emulator-manager-images/mac/06-custom-device.png)

-----

В этом окне настройте следующие параметры:

-   **Имя AVD** &ndash; уникальное имя для определения устройства. В примере снимка экрана выше задано имя **MyNexus**. Обратите внимание, что имя AVD не может содержать пробелы &ndash; в противном случае кнопка **ОК** будет недоступна.

-   **Устройство** &ndash; выберите профиль оборудования для эмуляции (например, **Nexus 5** или **Nexus 6**).

-   **Целевой уровень** &ndash; выберите уровень API Android для виртуального устройства. Значение этого параметра должен быть больше или равно минимальной версии Android вашего приложения.

-   **ЦП/ABI** &ndash; выберите **Google API Intel Atom (x86)**, чтобы API-интерфейсы Google были доступны в определении устройства.

-   **Обложка** &ndash; выберите внешний вид виртуального устройства. На примере снимка экрана выше выбрана обложка **HVGA** (пример обложки **HVGA** приведен на снимке экрана эмулятора в конце этой статьи).

-   **Параметры памяти** &ndash; как правило, по умолчанию для ОЗУ задано слишком высокое значение, поэтому в Windows выводится предупреждение: **Эмуляция ОЗУ выше 768 МБ может завершиться ошибкой**. Большинству пользователей рекомендуется использовать значение ОЗУ в 768 МБ (как показано на приведенном выше снимке экрана). Более высокие значения могут замедлить работу эмулятора.

-   **Использовать GPU узла** &ndash; если этот параметр выбран, для операций с графикой эмулятор использует графический процессор главного компьютера. Рекомендуется включить этот параметр, чтобы повысить производительность эмулятора. Дополнительные сведения о **параметрах эмуляции** см. в статье об [использовании параметров "Моментальный снимок" и "Использовать GPU узла"](https://android.stackexchange.com/questions/51739/what-is-snapshot-and-use-host-gpu-emulation-options-for).


Для остальных параметров можно оставить значения по умолчанию. Когда все будет готово, нажмите кнопку **ОК**, чтобы создать виртуальное устройства. Результаты конфигурации нового виртуального устройства приводятся в следующем диалоговом окне:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Диалоговое окно результатов после создания нового AVD](google-emulator-manager-images/win/08-create-results.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![Диалоговое окно результатов после создания нового AVD](google-emulator-manager-images/mac/07-create-results.png)

-----

Подробное описание свойств конфигурации, указанных в этом диалоговом окне, см. в статье о [свойствах профиля оборудования](https://developer.android.com/studio/run/managing-avds.html#hpproperties).
После нажатия кнопки **ОК** конфигурация нового устройства появится в списке существующих виртуальных устройств Android. На следующем снимке экрана показано, что в список было добавлено устройство **MyNexus**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Устройство MyNexus, добавленное в список устройств](google-emulator-manager-images/win/09-added-to-list-sml.png)](google-emulator-manager-images/win/09-added-to-list.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Устройство MyNexus, добавленное в список устройств](google-emulator-manager-images/mac/08-added-to-list-sml.png)](google-emulator-manager-images/mac/08-added-to-list.png)

-----

Новое пользовательское устройство также добавляется в раскрывающееся меню устройства:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Устройство MyNexus, добавленное в раскрывающееся меню](google-emulator-manager-images/win/10-available-custom-device-sml.png)](google-emulator-manager-images/win/10-available-custom-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Устройство MyNexus, добавленное в раскрывающееся меню](google-emulator-manager-images/mac/09-available-custom-device-sml.png)](google-emulator-manager-images/mac/09-available-custom-device.png)

-----


<a name="cloning" />

### <a name="cloning-a-device-definition"></a>Клонирование определения устройства

Чтобы создать пользовательское определение устройства, можно выбрать существующее определение устройства и *клонировать* его. Это может быть хорошим вариантом, если имеется существующее определение устройства, требующее внесения лишь незначительных изменений, после чего оно будет соответствовать вашим потребностям. Все доступные определения устройств указаны на вкладке **Определения устройств** в **диспетчере виртуальных устройств Android (AVD)**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Список доступных определений устройств](google-emulator-manager-images/win/11-device-definitions-sml.png)](google-emulator-manager-images/win/11-device-definitions.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Список доступных определений устройств](google-emulator-manager-images/mac/10-device-definitions-sml.png)](google-emulator-manager-images/mac/10-device-definitions.png)

-----

Предварительно настроенные устройства в этом списке изменить невозможно &ndash; можно изменять только созданные пользователем виртуальные устройства. Новое определение устройства можно наследовать от предварительно настроенного определения устройства. Для этого нужно выбрать определение устройства и нажать кнопку **Клонировать**. Например, при выборе определения **Nexus 5** и нажатии кнопки **Клонировать** открывается следующее диалоговое окно:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Диалоговое окно клонирования устройства](google-emulator-manager-images/win/12-clone-device-sml.png)](google-emulator-manager-images/win/12-clone-device.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Диалоговое окно клонирования устройства](google-emulator-manager-images/mac/11-clone-device-sml.png)](google-emulator-manager-images/mac/11-clone-device.png)

-----

На следующем снимке экрана имя устройства изменено на **Nexus 5 Custom**, и параметры устройства также изменяются для создания пользовательского определения устройства:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Устройство AVD Custom Nexus 5](google-emulator-manager-images/win/13-custom-nexus.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Устройство AVD Custom Nexus 5](google-emulator-manager-images/mac/12-custom-nexus-sml.png)](google-emulator-manager-images/mac/12-custom-nexus.png)

-----

При нажатии кнопки **Клонировать устройство** создается новое определение устройства, которое появится в списке **Определения устройств**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Nexus 5 Custom отображается как новое пользовательское определение устройства](google-emulator-manager-images/win/14-new-definition-sml.png)](google-emulator-manager-images/win/14-new-definition.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Nexus 5 Custom отображается как новое пользовательское определение устройства](google-emulator-manager-images/mac/13-new-definition-sml.png)](google-emulator-manager-images/mac/13-new-definition.png)

-----

Обратите внимание, что каждое созданное пользователем определение устройства отображается с зеленым значком, как показано выше. Это новое определение устройства можно использовать для создания новых устройств AVD, выбрав определение и нажав кнопку **Создать AVD**. Откроется диалоговое окно **Создание нового виртуального устройства Android (AVD)**. В следующем примере для нового виртуального устройства было автоматически сгенерировано имя **AVD\_for\_Nexus\_5\_Custom**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Создание устройства AVD из пользовательского определения устройства Nexus 5 Custom](google-emulator-manager-images/win/15-create-avd-sml.png)](google-emulator-manager-images/win/15-create-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Создание устройства AVD из пользовательского определения устройства Nexus 5 Custom](google-emulator-manager-images/mac/14-create-avd-sml.png)](google-emulator-manager-images/mac/14-create-avd.png)

-----

После нажатия кнопки **ОК** пользовательская конфигурация устройства появится в списке существующих виртуальных устройств Android. Кроме того, она будет добавлена в раскрывающееся меню устройства:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Новое пользовательское устройство AVD, добавленное в раскрывающееся меню устройства](google-emulator-manager-images/win/16-new-avd-sml.png)](google-emulator-manager-images/win/16-new-avd.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![Новое пользовательское устройство AVD, добавленное в раскрывающееся меню устройства](google-emulator-manager-images/mac/15-new-avd-sml.png)](google-emulator-manager-images/mac/15-new-avd.png)

-----

