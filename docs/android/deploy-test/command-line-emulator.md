---
title: "Эмулятор командной строки"
ms.topic: article
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.openlocfilehash: b8e8ec3de3afccab15d54f666974af678c1b8c33
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="command-line-emulator"></a>Эмулятор командной строки


## <a name="running-the-android-emulator-from-the-command-line"></a>Запуск эмулятора Android из командной строки

Для запуска эмулятора Android из командной строки можно использовать средство emulator, предоставляемое Android SDK. Это средство можно использовать для запуска эмулятора из терминала в OS X или из командной строки на компьютере Windows.

Для запуска определенного эмулятора Android выполните следующую команду из каталога средства в папке android SDK (например, C:\android-sdk-windows\tools):

В Windows

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

В macOS

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

Причина, по которой требуется настроить размер раздела, это предоставление эмулятору достаточно места для работы с платформой Xamarin.Android, установленной в эмуляторе, так как по умолчанию устанавливается эмулятор небольшого размера.

Дополнительные сведения о разных параметрах Android см. здесь: [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html).
