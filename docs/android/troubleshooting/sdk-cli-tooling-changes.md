---
title: "Изменения в Android SDK Tools"
description: "Изменения в управлении Android SDK установлен уровни API и Avd."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 69e9f08870a01c056951700978d07277af5edfa8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Изменения в Android SDK Tools

_Изменения в управлении Android SDK установлен уровни API и Avd._

## <a name="changes-to--android-sdk-tooling"></a>Изменения средств пакета SDK для Android

В современных версиях средств пакета SDK для Android, компания Google отменила существующих диспетчеров AVD и SDK in favour of новые _интерфейс командной строки_ средства (CLI). Первый **android** программа была удалена и диспетчеры GUI (графический интерфейс пользователя) в Visual Studio для Mac и более ранних версий Xamarin для Visual Studio больше не будет работать за пакет инструментов SDK Android версии.


![Android меню интегрированной среды разработки Visual Studio](sdk-cli-tooling-changes-images/android-ide-menu.png)

Попытка использовать **android** программы командной строки приведет к сообщение об ошибке следующего вида:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Таким образом необходимо использовать средства интерфейса командной строки для управления и обновления эмуляторов и пакета SDK для Android.

### <a name="cli-tools"></a>Средства интерфейса командной строки

Следующие программы теперь составляют интерфейс командной строки для средства пакета SDK для Android:

#### <a name="sdkmanager"></a>sdkmanager

**Добавлено в:** пакет инструментов SDK Android 25.2.3 (ноябрь 2016 г.) и более поздних версий.

Имеется новая программа, которая называется **sdkmanager** в **Сервис/bin** папку пакета SDK для Android. Это средство используется для сохранения пакета SDK для Android в командной строке. Дополнительные сведения об использовании этого инструмента см. в разделе [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Добавлено в:** пакет инструментов SDK Android 25.3.0 (марта 2017 г.) и более поздних версий.

Имеется новая программа, которая называется **avdmanager** в **Сервис/bin** папку пакета SDK для Android. Это средство используется для поддержания AVD для эмулятора Google Android. Дополнительные сведения об использовании этого инструмента см. в разделе [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Понижение уровня

Вы можете перейти к **пакет инструментов SDK Android** версии путем установки предыдущей версии пакета SDK для Android из [веб-сайте разработчика Android](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>С помощью старого графического пользовательского интерфейса

Можно по-прежнему использовать исходный графического пользовательского интерфейса, выполнив **android** программы внутри вашей **средства** при условии, что находятся в папке **пакет инструментов SDK Android** версии **25.2.5**  или ниже.


## <a name="related-links"></a>Связанные ссылки

- [Установка пакета SDK для Android](~/android/get-started/installation/android-sdk.md)
- [Основные сведения об уровнях Android API](~/android/app-fundamentals/android-api-levels.md)
- [Заметки о выпуске (Google) средств пакета SDK](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
