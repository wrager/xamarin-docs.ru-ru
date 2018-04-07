---
title: Некоторые конкретных ошибок лицензирования
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: D26BDF2D-923B-4BC1-841A-74583155DB71
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 8592fe5381c974e999477d0ef6ca6ebdd8b38cc4
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2018
---
# <a name="some-specific-licensing-errors"></a>Некоторые конкретных ошибок лицензирования

> [!IMPORTANT]
> Приведенные ниже сведения не применяется для пользователей MSDN, поскольку они являются проблем, связанных с предыдущих версий лицензий Xamarin. Если вы являетесь пользователем MSDN и возникают ошибки, аналогичные ниже, повторите [обновление до последней версии Xamarin](https://developer.xamarin.com/recipes/cross-platform/ide/change_updates_channel/) & ссылаться на это [описываются варианты лицензирования](~/cross-platform/get-started/requirements.md).



## <a name="invalid-license"></a>«Недопустимый лицензия»

> Лицензия недействительна. Можно повторно активируйте Xamarin.Android (XA9999) не удалось определить выпуск лицензией. (XA9010)

Эти сообщения могут появляться в нескольких разных обстоятельствах.

-   Текущая лицензия на диске может быть из несинхронизированных текущую информацию пользователя на компьютере. [Обновление файлов лицензий](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) будет устранить эту проблему, во многих случаях.

-   На компьютере может быть 2 конфликтующие повторяющиеся активаций. Этот тип лицензии больше не используется Xamarin. Если возникают проблемы, которые могут быть об этой ошибке обратитесь [поддержка по электронной почте](https://www.xamarin.com/support).

-   Учетной записи Xamarin может не еще получить приглашение на лицензию Xamarin. См. также: [группы управления лицензиями](~/cross-platform/troubleshooting/legacy-licenses/team-management.md).

## <a name="failed-to-load-android-entitlements"></a>«Не удалось загрузить Android прав»

> Ошибка Mono.VisualStudio.Shell.ShellPackage: 0: не удалось загрузить Android прав: недопустимой лицензией. Выполните повторно активировать Xamarin.Android (XA9999) Mono.VisualStudio.Shell.ShellPackage ошибки: 0: не удалось обновить лицензию: недопустимой лицензией. Можно повторно активировать Xamarin.Android (XA9999)

Это более старая версия проблемы «Недействительную лицензию» выше. Применяются те же шаги по устранению неполадок.

## <a name="this-version-was-released-after-your-subscription-expired"></a>«Эта версия был выпущен после истечения срока действия подписки»

> Ошибка XA9000: Эта версия был выпущен после истечения срока действия подписки (11/11/2014 17:11:41: 00). (XA9000) Ошибка XA9010 (Mobile.Android.Utilities): не удалось определить выпуск лицензией. (XA9010) (Mobile.Android.Utilities)

Эти сообщения обычно отображаются в двух случаях:

-   Лицензии на диске устарели. [Обновление файлов лицензий](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md) будет устранить эту проблему, во многих случаях.

-   Xamarin подписки больше не активна и более поздней, чем дата окончания срока действия подписки текущую установленную версию Xamarin. В этом случае правильным решением является [понизить](http://kb.xamarin.com/customer/portal/articles/1699777) версию Xamarin, выпущенной истечения срока действия подписки. Вы можете отправить сообщение электронной почты для [ hello@xamarin.com ](mailto:hello@xamarin.com) для запроса последних допустимую версию для вашей подписки. После понижения по-прежнему отображается сообщение, обязательно обновите файлы лицензии слишком.

## <a name="additional-references"></a>Дополнительные ссылки

-   [Как обновить лицензии](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)
-   [Способ перехода Xamarin](http://kb.xamarin.com/customer/portal/articles/1699777-downgrading)
-   [Список кодов ошибок Xamarin.Android](~/android/troubleshooting/errors.md)
-   [Список кодов ошибок MonoTouch (Xamarin.iOS)](~/ios/troubleshooting/mtouch-errors.md)

### <a name="next-steps"></a>Следующие шаги
Для получения дополнительной помощи, свяжитесь с нами, или если эта проблема остается даже после использования указанные выше сведения см. в разделе [какие варианты поддержки доступны для Xamarin?](~/cross-platform/troubleshooting/support-options.md) сведения о параметрах контактов, предложения, а также как файл новую ошибку, при необходимости.
