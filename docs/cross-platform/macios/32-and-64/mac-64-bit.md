---
title: Обновление приложений единой Xamarin.Mac на 64-разрядную версию
description: В этом руководстве описывается обновления Xamarin.Mac целевой 64-разрядных приложений
ms.prod: xamarin
ms.assetid: C3810A74-539C-4FFB-B47F-68CA5F7BCDAD
ms.technology: xamarin-cross-platform
author: bradumbaugh
ms.author: brumbaug
ms.date: 02/22/2018
ms.openlocfilehash: e365fe1af47338f41aebe4bc0d81d289466a9b6c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="updating-xamarinmac-unified-applications-to-64-bit"></a>Обновление приложений единой Xamarin.Mac на 64-разрядную версию

Начиная с января 2018, Apple требует, чтобы новые [отправок Mac App Store целевой 64-разрядных](https://developer.apple.com/news/?id=06282017a). Приложения, уже доступные в магазине приложений Mac должны обновляться целевой 64-разрядных по 2018 июня.

**Файл** > **New** Xamarin.Mac шаблон проекта создает 64-разрядных приложений по умолчанию, поэтому все недавно созданных приложения уже 64-разрядными платформами и менять не требуется.

## <a name="targeting-64-bit"></a>Предназначенных для 64-разрядной

1. Откройте **параметры проекта** окна для вы являетесь Xamarin.Mac приложения:

   ![Контекстные меню для проекта](mac-64-bit-images/1-contextual_menu-vsmac.png "контекстные меню для проекта")

2. Выберите **построения Mac** и задайте **Поддерживаемые архитектуры** для **x86\_64**:

   [![Установка поддерживаемых архитектур x86_64](mac-64-bit-images/2-project_options-vsmac.png "параметру x86_64 Поддерживаемые архитектуры")](mac-64-bit-images/2-project_options-vsmac-large.png#lightbox)

3. Если ваше приложение имеет все внешние зависимости, такие как собственные ссылки или привязки проектов, их необходимо обновите до целевой 64-разрядной.

### <a name="errors"></a>Ошибки

При первом построении и запуске приложения с поддержкой 64-разрядной, могут возникнуть ошибки ссылку из среды выполнения или clang проблемы. Эти ошибки могут возникать, если сторонние зависимости — например, собственных ссылок Xamarin.Mac или проекты привязок, или вручную загрузить системных платформ — не были обновлены на 64-разрядную версию.

> [!TIP]
> Преобразование проекта в 64-разрядный существенных изменений и косвенно проявляются различных ошибок программирования. В частности может изменить размер и выравнивание структуры данных, которые может повлиять на подписей p/invoke и машинный код, связанный в проекте. Просмотрите все предупреждения при построении заданному и тестирования приложения throughly впоследствии для выявления потенциальных проблем.

#### <a name="example-error-resulting-from-a-dynamically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Пример ошибки, возникающие в результате динамически скомпонованные сторонней зависимости, не предназначенных для 64-разрядных:

```console
ld : warning : ignoring file PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary, 
file was built for i386 which is not the architecture being linked (x86_64): 
PATH/ThirdPartyLibrary.framework/ThirdPartyLibrary 
```

Эта ошибка может идти во время выполнения, `dlopen` возврат `IntPtr.Zero` вместо ожидаемого дескриптор.

#### <a name="example-error-resulting-from-a-statically-linked-third-party-dependency-that-does-not-target-64-bit"></a>Пример ошибки, возникающие в результате статически компонуемые сторонней зависимости, не предназначенных для 64-разрядных:

```console
Undefined symbols for architecture x86_64:
  "_LibraryFunction", referenced from:
     -u command line option
ld: symbol(s) not found for architecture x86_64 
```

Для успешного построения и запуска, обновите эти зависимости на 64-разрядную и перекомпилировать приложение.

