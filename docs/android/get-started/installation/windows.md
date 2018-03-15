---
title: "Установка в Windows"
description: "В этом руководстве описаны действия по установке Xamarin.Android для Visual Studio в Windows, а также содержатся сведения о настройке Xamarin.Android для создания первого приложения Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2BE4D5AD-D468-B177-8F96-837D084E7DE1
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/01/2018
ms.openlocfilehash: 7cf21e75c9ae2f3c27b07cb20f1044779b42b06b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="windows-installation"></a>Установка в Windows

_В этом руководстве описаны действия по установке Xamarin.Android для Visual Studio в Windows, а также содержатся сведения о настройке Xamarin.Android для создания первого приложения Xamarin.Android._


## <a name="overview"></a>Обзор

Поскольку теперь Xamarin является бесплатным компонентом всех выпусков Visual Studio и не требует отдельной лицензии, для скачивания и установки средств Xamarin.Android можно использовать установщик Visual Studio.
(Выполнять действия по установке вручную и лицензированию, которые требовались для более ранних версий Xamarin.Android, больше не нужно.) В этом руководстве рассматриваются следующие темы:

-   Настройка пользовательских расположений для пакета Java Development Kit, пакета SDK для Android и пакета NDK для Android.

-   Запуск диспетчера пакетов SDK для Android для скачивания и установки дополнительных компонентов пакета SDK для Android.

-   Подготовка устройства или эмулятора Android для отладки и тестирования.

-   Создание первого проекта приложения Xamarin.Android.

После выполнения действий в этом руководстве ваша рабочая установка Xamarin.Android будет интегрирована в Visual Studio, и вы будете готовы приступить к созданию первого приложения Xamarin.Android.

## <a name="installation"></a>Установка

Подробные сведения об установке Xamarin для использования с Visual Studio в Windows см. в руководстве [Установка в Windows](~/cross-platform/get-started/installation/windows.md).


## <a name="configuration"></a>Конфигурация

Для создания приложений в Xamarin.Android используется пакет Java Development Kit (JDK) и пакет SDK для Android. Во время установки установщик Visual Studio помещает эти средства в расположения по умолчанию и настраивает среду разработки с соответствующей конфигурацией путей. Чтобы просмотреть или изменить эти расположения, последовательно выберите **Сервис > Параметры > Xamarin > Параметры Android**:

![Снимок экрана диалогового окна параметров Xamarin Android](windows-images/07-settings.png)

В большинстве случаев эти расположения по умолчанию будут работать без дальнейших изменений. Однако в Visual Studio можно настроить пользовательские расположения для этих средств (например, если пакеты Java JDK, SDK для Android или NDK установлены в другом месте). Нажмите кнопку **Изменить** рядом с путем, который требуется изменить, а затем перейдите в новое расположение.

Xamarin.Android использует пакет [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html), который необходим при разработке для API уровня 24 или выше (пакет JDK 8 также поддерживает уровни API ниже 24). При разработке специально для уровня API 23 или ниже можно продолжать использовать пакет [JDK 7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html).

> [!IMPORTANT]
> Xamarin.Android не поддерживает пакет JDK 9.


### <a name="android-sdk-manager"></a>Диспетчер Android SDK

Android использует несколько параметров уровня API Android для определения совместимости приложения в разных версиях Android (дополнительные сведения об уровнях API Android см. в статье [Общие сведения об уровнях API Android](~/android/app-fundamentals/android-api-levels.md)).
В зависимости от того, с какими уровнями API Android вы будете работать, может потребоваться скачать и установить дополнительные компоненты пакета SDK для Android. Кроме того, может потребоваться установить дополнительные средства и образы эмулятора из пакета SDK для Android. Для этого следует использовать **диспетчер пакетов SDK для Android**. Запустите **диспетчер пакетов SDK для Android**, выбрав **Сервис > Android > Диспетчер пакетов SDK для Android**:

[![Запуск диспетчера пакетов SDK для Android](windows-images/08-sdk-manager-sml.png)](windows-images/08-sdk-manager.png#lightbox)

Диспетчер пакетов SDK для Android установлен в Visual Studio по умолчанию:

![Снимок экрана с примером диспетчера пакетов SDK для Android Google](windows-images/09-google-sdk-manager.png)

С помощью диспетчера пакетов SDK для Android Google можно устанавливать пакеты инструментов SDK для Android до версии 25.2.3. Однако если необходимо использовать более позднюю версию пакета инструментов SDK для Android, нужно установить подключаемый модуль диспетчера пакетов SDK для Android Xamarin для Visual Studio (доступен в Visual Studio Marketplace). Это необходимо, так как автономный диспетчер пакетов SDK Google был объявлен нерекомендуемым в версии 25.2.3 пакета инструментов SDK для Android. 

Дополнительные сведения об использовании диспетчера пакетов SDK для Android Xamarin см. в статье [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md).


### <a name="android-emulator"></a>Эмулятор Android

При отсутствии физического устройства Android для тестирования приложения подойдет эмулятор Android. Дополнительные сведения об эмуляторе Android Google см. в статье [Эмулятор SDK для Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Эмулятор Android Google использует HAXM (Hardware Accelerated Execution Manager) от Intel, который может конфликтовать с технологиями виртуализации, применяемыми в других эмуляторах. Ниже перечислены три основные технологии виртуализации:

-   **Hyper-V** (используется эмулятором Visual Studio для Android и эмулятором Windows Phone) 

-   **Virtual Box** (используется Genymotion)

-   **Intel HAXM** (используется эмулятором SDK для Android Google) 

Так как процессоры компьютера разработки поддерживают только одну технологию виртуализации одновременно, рекомендуется использовать только одну из них на компьютере, на котором ведется разработка.

<a name="device" />

### <a name="android-device"></a>Устройство Android

Если у вас есть физическое устройство Android для тестирования, настройте его для использования в разработке. Сведения о настройке устройства Android для разработки, его подключении к компьютеру для запуска и отладки приложений Xamarin.Android см. в статье [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md).


## <a name="create-an-application"></a>Создание приложения

Теперь, когда вы установили Xamarin.Android, можно запустить Visual Studio для создания проекта. Выберите **Файл > Создать > проект**, чтобы приступить к созданию приложения:

![Создание нового проекта](windows-images/10-new-project.png)

В диалоговом окне **Новый проект** в разделе **Шаблоны** выберите **Android** и на правой панели щелкните **Пустое приложение (Android)**. Введите имя приложения (на снимке экрана ниже приложение называется **MyApp**), а затем нажмите кнопку **ОК**:

[![Снимок экрана диалогового окна "Новый проект" для создания пустого приложения Android](windows-images/11-first-app-sml.png)](windows-images/11-first-app.png#lightbox)

Вот и все! Теперь вы готовы использовать Xamarin.Android для создания приложений Android!


## <a name="summary"></a>Сводка

В этой статье вы узнали, как настроить и установить платформу Xamarin.Android в Windows, как (необязательно) настроить Visual Studio с пользовательскими расположениями для пакета Java JDK и пакета SDK для Android, как запустить диспетчер пакетов SDK для установки дополнительных компонентов пакета SDK для Android, как настроить устройство или эмулятор Android и как приступить к созданию первого приложения.

Следующим шагом является изучение учебников [Привет, Android](~/android/get-started/hello-android/index.md), содержащих сведения о создании работающего приложения Xamarin.Android.


## <a name="related-links"></a>Связанные ссылки

- [Скачать Visual Studio 2012](https://www.visualstudio.com/vs/)
- [Установка инструментов Visual Studio для Xamarin](~/cross-platform/get-started/installation/windows.md)
- [Требования к системе](~/cross-platform/get-started/requirements.md)
- [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md)
- [Эмулятор SDK для Android](~/android/get-started/installation/android-emulator/index.md)
- [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md)
