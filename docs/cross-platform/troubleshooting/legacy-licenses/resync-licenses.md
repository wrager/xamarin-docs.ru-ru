---
title: Как вручную синхронизировать лицензий Xamarin?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D0BD93E9-3A1F-4E5B-8EE8-36ADC33DCFE4
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b06a1a7d525c91d7c3973b2b02d3d2835ce482f9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-manually-resynchronize-xamarin-licenses"></a>Как вручную синхронизировать лицензий Xamarin?

> [!IMPORTANT]
> В этом руководстве не применяется к большинству пользователей MSDN, так как они не требуются для владельцем или входить в Xamarin учетные записи, если используется [хранения компоненты Xamarin](https://components.xamarin.com/) или [Visual Studio для Mac (Mac)](~/cross-platform/get-started/requirements.md).




## <a name="overview"></a>Обзор

Обычно данные лицензии будут повторно синхронизировать с сервером лицензий Xamarin автоматически при запуске интегрированной среды разработки. Например обновления лицензий, возврат к предыдущей версии и обновления обычно автоматически появится после перезапуска интегрированной среды разработки. Лицензия сведений, отображаемых в Интегрированной среде разработки должен соответствовать сведения, отображаемые на ваш [странице учетной записи](https://store.xamarin.com/account/my/subscription/computers). Если данные по-прежнему несоответствующие, можно сначала появилось верны сведений учетной записи хранилища и затем вручную повторно синхронизировать лицензий выход, удалив файлы лицензии на диске и снова войдя в.

### <a name="note-for-ios-developers-on-windows"></a>Примечание для разработчиков решений iOS в Windows

Разработчики iOS в Windows также может потребоваться выполнить следующие действия для Visual Studio для Mac; даже если не разрабатываемых непосредственно на узле парной Mac сборки.

## <a name="quick-manual-refresh-steps"></a>Быстрое обновление вручную действия

Это быстрый процесс пропускает последующего удаления файлов лицензий на диске, который может быть достаточно в некоторых случаях. 

1.  Выход из вашей учетной записи Xamarin в среде IDE:
    -   Visual Studio для Mac
        -   Правом верхнем углу экрана приветствия
        -   **Visual Studio для Mac > учетной записи** (Mac)
        -   **Сервис > учетной записи** (Windows)
    -   Visual Studio
        -   **Сервис > учетной записи Xamarin**
2.  Войдите в вашей учетной записи Xamarin в Интегрированной среде разработки.

## <a name="extended-manual-license-refresh-steps"></a>Расширенные лицензии вручную действия обновления

1.  Выйдите из учетной записи Xamarin в среде IDE. 
2.  Выйти из интегрированной среды разработки.
3.  Проверьте, что сведения об учетной записи хранилища указано правильно. В частности проверять компьютеры с одинаковыми именами в [страницы компьютеров](https://store.xamarin.com/account/my/subscription/computers).

4.  Если вы видите пару дублирующиеся имена компьютеров, используйте **деактивировать** раскрывающееся меню элемента, удаляемого _оба_ члены пары:
    
    ![Лицензия "->" Отключить на https://store.xamarin.com/account/my/subscription/computers ] (resync-licenses-images/deactivate.png "используется элемент деактивировать раскрывающееся меню для удаления обоих участников пары")

5.  Удалите все оставшиеся копии файлов лицензий все еще присутствует на диске.
    -   Windows

        Удалите все из следующих папок:
        -   `%PROGRAMDATA%\Mono for Android`
        -   `%PROGRAMDATA%\MonoTouch`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\Mono for Android`
        -   `%LOCALAPPDATA%\VirtualStore\ProgramData\MonoTouch`

        В установках Windows по умолчанию:
        -   `%PROGRAMDATA%` будет расширен, чтобы `C:\ProgramData`
        -   `%LOCALAPPDATA%` будет расширен, чтобы `C:\Users\%USERNAME%\AppData\Local`

        `VirtualStore` Копии лицензий, которые создаются автоматически с Windows в определенных сценариях. Если существуют копии VirtualStore, они будут интерпретироваться _вместо_ лицензий в `%PROGRAMDATA%`.

    -   Mac

        Удалите все из следующих папок:

        -   `~/Library/MonoAndroid`
        -   `~/Library/MonoTouch`
        -   `~/Library/Xamarin.Mac`

        Можно получить доступ к этим путям в **Finder** , выбрав **Go > перейдите в папку** меню и их вставки в диалоговом окне. Поиска автоматически заменяет ~ тильда с папкой пользователя.

6.  Откройте Visual Studio или Visual Studio для Mac и журнала обратно в вашей учетной записи Xamarin.

## <a name="supplementary-information-individual-license-file-locations"></a>Дополнительные сведения: расположение отдельных лицензий

### <a name="windows"></a>Windows

В Windows отдельных лицензий файлы хранятся в следующих местах:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.licx`

Лицензии для *пробной* подписки названы по-разному:

-   Xamarin.Android:  
     `%PROGRAMDATA%\Mono for Android\License\monoandroid.trial.licx`
-   Xamarin.iOS:  
     `%PROGRAMDATA%\MonoTouch\License\monotouch.trial.licx`

### <a name="mac"></a>Mac

На Mac отдельных лицензий файлы хранятся в следующих местах:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.v2`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License`

Лицензии для *пробной* подписки названы по-разному:

-   Xamarin.Android:  
     `~/Library/MonoAndroid/License.trial`
-   Xamarin.iOS:  
     `~/Library/MonoTouch/License.trial`
-   Xamarin.Mac:  
     `~/Library/Xamarin.Mac/License.trial`

## <a name="additional-troubleshooting-steps"></a>Дополнительные действия по устранению неполадок

-   Проверьте наличие любого символа ASCII в свое имя пользователя, имя компьютера или в каком-либо полностью уточненное путей к файлам лицензий.

-   [Обратитесь в службу поддержки разработчиков на Xamarin](http://xamarin.com/support)
