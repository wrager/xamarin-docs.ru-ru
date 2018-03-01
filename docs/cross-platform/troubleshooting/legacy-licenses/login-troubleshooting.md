---
title: "Почему не удается входа на Xamarin в Visual Studio или Visual Studio для Mac?"
ms.topic: article
ms.prod: xamarin
ms.assetid: 6EF2B553-5DF9-41CC-B68F-77A7F64572FA
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: b50969e4c1d75c0bd79c08223dd959241dcb229a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="why-cant-i-log-into-xamarin-in-visual-studio-or-visual-studio-for-mac"></a>Почему не удается входа на Xamarin в Visual Studio или Visual Studio для Mac?

> [!IMPORTANT]
> В этом руководстве не применяется к большинству пользователей MSDN, так как они не требуются для владельцем или входить в Xamarin учетные записи, если используется [хранения компоненты Xamarin](https://components.xamarin.com/) или [Visual Studio для Mac (Mac)](~/cross-platform/get-started/requirements.md). Владельцы лицензия MSDN должны ссылаться на это [описываются варианты лицензирования](~/cross-platform/get-started/requirements.md) вместо него.



## <a name="overview"></a>Обзор
Существует несколько распространенных причин, которые могут помешать ведения журналов в учетной записи Xamarin в Интегрированной среде разработки. Ниже приводится описание известных проблем и ошибок.

### <a name="finding-the-login-screen"></a>Поиск экран входа

Справочник по экраны входа находятся здесь:

- Visual Studio для Mac
   - Правом верхнем углу экрана приветствия
   - **Visual Studio для Mac > учетной записи** (Mac)
   - **Сервис > учетной записи** (Windows)
- Visual Studio
   - **Сервис > учетной записи Xamarin**

## <a name="the-ide-is-connecting-but-the-account-screen-isnt-showing-correct-login-information"></a>Подключение интегрированной среды разработки, но экран учетная запись не отображается правильные учетные данные

Обычно эта проблема разрешается путем повторную синхронизацию вручную лицензии.
Следуйте указаниям, приведенным в этой статье: [инструкции вручную лицензий Xamarin resyncronize?](~/cross-platform/troubleshooting/legacy-licenses/resync-licenses.md)

## <a name="invalid-account-information"></a>Недопустимая информация учетной записи

Если перейти на веб-сайте Xamarin [страницы входа](https://store.xamarin.com/Login?from=%2faccount%2f), выполните попытку входа в систему с учетными данными текущего.
На странице также имеются ссылки для сброса пароля и создать новую учетную запись.

## <a name="account-is-valid-but-the-ide-cant-connect"></a>Учетная запись действительна, но интегрированной среды разработки не удается подключиться

Обычно это вызывается брандмауэра или другие параметры безопасности блокировать IDE доступ необходимые конечные точки.
Серверы активации необходимо доступ следующее:

> Activation.xamarin.com store.xamarin.com auth.xamarin.com

Однако несколько конечных точек важны для общей разработки процессов, таких как обновления, получение пакетов NuGet и т. д. По этой причине рекомендуется убедитесь, что *все* добавляются конечные точки из [руководство по выбору конфигурации брандмауэра Xamarin](~/cross-platform/get-started/installation/firewall.md).

### <a name="ios-in-xamarin-studio-windows"></a>операций ввода-вывода в Xamarin Studio Windows
Разработка приложений iOS не поддерживается в Xamarin Studio для Windows. При доступе к экране входа, лицензии на iOS не отображается.

Вместо этого имени входа через Visual Studio или Xamarin Studio (Mac) с лицензией прежних версий. Обратите внимание, что пользователям MSDN в Windows не требуется выполнить вход.

## <a name="additional-support"></a>Дополнительная поддержка

При указанных выше сценариев не описаны ситуации или устранить проблему, обратитесь к этим [варианты поддержки](https://www.xamarin.com/support).
