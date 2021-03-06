---
title: Настройка эмулятора Android
description: В этом разделе описано, как подготовить эмулятор Google Android для тестирования приложения. Здесь объясняется, как ускорить эмулятор для достижения максимальной производительности, и демонстрируется использование диспетчера эмуляторов для создания и настройки виртуальных устройств.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 215e298068b7a3a23b2e469e923f172c8303bbcb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-setup"></a>Настройка эмулятора Android

_В этом разделе описано, как подготовить эмулятор Google Android для тестирования приложения. Здесь объясняется, как ускорить эмулятор для достижения максимальной производительности, и показано, как использовать диспетчер эмуляторов для создания и настройки виртуальных устройств._


## <a name="overview"></a>Обзор

Для моделирования разнообразных устройств эмулятор SDK для Android Google можно запускать в различных конфигурациях. Каждая из этих конфигураций создается в виде _виртуального устройства_. В этом руководстве вы узнаете, как ускорить эмулятор Android для повышения производительности и использовать диспетчер эмуляторов Android Xamarin или устаревший диспетчер эмуляторов Google для создания виртуальных устройств.


> [!NOTE]
> В Android SDK Tools **26.0.1** и более поздних версий компания Google исключила поддержку существующих диспетчеров AVD и SDK, заменив их новыми средствами интерфейса командной строки (CLI). В связи с этим изменением теперь вместо диспетчеров эмуляторов или пакетов SDK для средств Android 26.0.1 и более поздних версий используются диспетчеры устройств или пакетов SDK для Xamarin. (Дополнительные сведения о диспетчере пакетов SDK для Xamarin SDK см. в статье [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md).)


## <a name="sections"></a>Разделы

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Аппаратное ускорение](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

Сведения о подготовке компьютера для максимальной производительности эмулятора Google Android. Поскольку без аппаратного ускорения эмулятор Google Android может работать слишком медленно, перед его использованием на компьютере рекомендуется включить аппаратное ускорение.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Диспетчер устройств Android Xamarin](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

Сведения об использовании Xamarin Android Device Manager для создания и настройки виртуальных устройств эмулятора Google Android. **Диспетчер устройств Android Xamarin** (сейчас доступна предварительная версия) предназначен для замены устаревшего диспетчера эмуляторов Google. Если вы работаете в Android Oreo 8.0 или более поздних версиях, вместо диспетчера эмуляторов Google необходимо использовать диспетчер устройств Android Xamarin.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Диспетчер эмуляторов Google](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

Сведения об использовании устаревшего диспетчера эмуляторов Google для создания и настройки виртуальных устройств эмулятора Google Android. Можно продолжать использовать эмулятор Android Google с исходным диспетчером эмуляторов Google, оставив средства пакета SDK для Android версии 25.2.5 или более ранней.

После настройки эмулятора SDK для Android см. статью [Эмулятор Google Android ](~/android/deploy-test/debugging/android-sdk-emulator/index.md), чтобы узнать, как запустить эмулятор и использовать его для тестирования и отладки приложения.
