---
title: Какие драйверы USB требуется отладка Android в Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/30/2018
ms.openlocfilehash: 7b3a4c2f807839897a099959fe3a6ea9ec25df78
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732753"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Какие драйверы USB требуется отладка Android в Windows?

## <a name="finding-usb-drivers"></a>Поиск драйверов USB

Для отладки на устройстве с Android при разработке в Windows; необходимо установить совместимый драйвер USB. Диспетчер Android SDK включает «Драйвер Google USB» по умолчанию, что добавлена поддержка для устройств хранилища, как описано здесь: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Другие устройства требуют USB драйверов, в частности опубликованных производителем устройства. Некоторые ссылки для наиболее распространенных производителей включаются в этом руководстве: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Альтернативные варианты

В зависимости от manfacturer может быть трудно отследить требуется точное драйвер USB. Некоторые альтернативные решения для тестирования приложений Android, разработанных в том числе с помощью эмулятора Android или внешние тестирования службы Windows. Некоторые из них указаны ниже:

- [Приложение центра тестирования](https://docs.microsoft.com/appcenter/test-cloud/) — тестирование службы выполняются на сотнях real Android устройств облака.

- [Эмулятор Visual Studio для Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Отладка с помощью эмулятора Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

