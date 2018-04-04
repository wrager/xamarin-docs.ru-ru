---
title: Какие пакеты Android SDK следует установить?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/02/2018
ms.openlocfilehash: d1fc8a1c511e86615108d1df841c28256f23191f
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="which-android-sdk-packages-should-i-install"></a>Какие пакеты Android SDK следует установить?

Установка пакета SDK для Android не содержит автоматически все минимальный набор обязательных пакетов для разработки. Хотя отдельные разработчик должен отличаться, следующие пакеты обычно потребуются для разработки с использованием Xamarin.Android:

## <a name="tools"></a>Инструменты

Установите новейшие средства в папке средств в диспетчер пакета SDK:

- Средства пакета SDK для Android
- Пакет инструментов платформы Android SDK
- Средства построения пакета SDK для Android

## <a name="android-platforms"></a>Android платформы

Установка версии Android, которые заданы как минимум & целевой «Платформа SDK». 

Примеры

- Целевой API 23
- Минимальная API 23

Необходимо установить пакет SDK платформы для API 23 только

- Целевой API 23
- Минимальная API 15

Необходимо установить пакет SDK платформы для API 15 и 23. Обратите внимание, что не устанавливать уровни API между минимальным и целевым (даже если backporting на этих уровнях API).

## <a name="system-images"></a>Образы системы
Это только требуется, если вы хотите использовать эмуляторы Android out of box от Google. 

- [Настройка эмулятора по умолчанию](~/android/get-started/installation/android-emulator/index.md)

- [Как ускорить эмулятор по умолчанию](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Дополнения
Android SDK дополнения обычно не являются обязательными; Однако полезно следует учитывать их, так как они могут потребоваться в зависимости от вашей варианта использования.

## <a name="further-reading"></a>Дополнительные сведения
Следующее руководство рассматриваются эти параметры и содержит более подробные сведения о различных пакетах SDK manager имеет доступного: [диспетчер Android SDK Manager руководство по установке](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

