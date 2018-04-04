---
title: Приступая к работе с Objective-c.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 96afe6bbfd1d9c6f4fd5ca3b7358ef0b89da30bb
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-objective-c"></a>Приступая к работе с Objective-c.

Это странице началу работы для Objective-C, описываются основы работы для всех поддерживаемых платформ.


## <a name="requirements"></a>Требования

Для использования с целью C внедрения .NET необходимо иметь Mac под управлением:

* macOS 10.12 (Сьерра) или более поздней версии
* Xcode 8.3.2 или более поздней версии
* [Моно 5.0](http://www.mono-project.com/download/)

Можно установить [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) для редактирования и компиляции кода C#.


Примечания.

* Более ранние версии macOS, Xcode и Mono _могут_ работать, но проверялась, а не поддерживается;
* Создание кода может выполняться в Windows, но поддерживается только для этой компиляции на компьютере Mac, где установлена Xcode;


## <a name="installation"></a>Установка

Следующим шагом является загрузка и установка .NET внедрение на компьютере Mac.

* [Package](https://dl.xamarin.com/embeddinator/Xamarin.Embeddinator-4000-0.2.0.79.pkg)
* [Заметки о выпуске](https://github.com/mono/Embeddinator-4000/tree/master/docs/releases)

В качестве альтернативы можно построить из наших [репозитории](https://github.com/mono/Embeddinator-4000/tree/objc), см. [дополнение](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) документа инструкции.

Установщик не установщика Стандартная pkg на основе:

![Введение установщика](images/install1.png)
![установщик устанавливает тип](images/install2.png)
![Сводка установщика](images/install3.png)

После установленных с помощью установщика, после запуска новый сеанс терминала, можно использовать `objcgen` команды.
В противном случае всегда можно запустить средство помощью абсолютный путь к нему: `/Library/Frameworks/Xamarin.Embeddinator-4000.framework/Commands/objcgen`.

## <a name="platforms"></a>Платформы

Objective-C — это язык, чаще всего используется для создания приложений для macOS, iOS, tvOS и watchOS; и embeddinator поддерживает все эти платформы. Работа с каждой из платформ подразумевает ряд ключевых различий и пояснения [здесь](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Создание приложения macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) является простым, поскольку не включает в себя столько дополнительные действия, такие как Настройка удостоверений, provisining профили, симуляторов и устройств. Вы, рекомендуется начать с документом macOS перед преобразованием для операций ввода-вывода.

### <a name="ios--tvos"></a>iOS и tvOS

Убедитесь, что вы уже настроенные для разработки приложений iOS перед попыткой создания с помощью embeddinator. [Инструкциям](~/tools/dotnet-embedding/get-started/objective-c/ios.md) предполагается, что вы уже выполнил успешное создания и развертывания приложения iOS с компьютера.

Поддержку для tvOS, аналогичен работе операций ввода-вывода, с использованием только проекты tvOS интегрированные среды разработки (Visual Studio и Xcode) вместо проекты iOS.

> [!NOTE]
> Примечание: Поддержка watchOS будут доступны в будущих выпусках и будет очень похожа на iOS и tvOS.


## <a name="further-reading"></a>Дополнительные сведения

* [Внедрение .NET функций, специфичных для Objective-c.](~/tools/dotnet-embedding/objective-c/index.md)
* [Советы и рекомендации для Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Ограничения внедрения .NET](~/tools/dotnet-embedding/limitations.md)
* [Вклад в проект с открытым исходным кодом](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Коды ошибок и описания](~/tools/dotnet-embedding/errors.md)
* [Целевые платформы](~/tools/dotnet-embedding/objective-c/platforms.md)


## <a name="related-links"></a>Связанные ссылки

- [Пример Weather (iOS и macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
