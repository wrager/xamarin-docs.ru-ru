---
title: Объединить Google Play служб компонентов и NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5D962EB4-2CB3-4B7D-9D77-889DEACDAE02
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: e0ba5ee9417917b834ab060a94f72d1f071b4912
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="unifying-google-play-services-components-and-nuget"></a>Объединить Google Play служб компонентов и NuGet

### <a name="history"></a>Журнал

Раньше несколько компонентов служб воспроизвести Google и пакеты NuGet:

-   Службы Google Play (Froyo)
-   Службы Google Play (Gingerbread)
-   Google Play служб (ICS)
-   Службы Google Play (JellyBean)
-   Службы Google Play (KitKat)

Фактически только поставляется с двумя .jar Google файлы для службы Google Play:

-   `google-play-services-froyo.jar`
-   `google-play-services.jar`

О расхождении существовал, поскольку наш инструментарий не сказали правильно `aapt.exe` ресурсов максимальный уровень API был для данного приложения. Это значило, мы получили ошибки компиляции, если было произведено с помощью привязки службы Google Play (KitKat) на более низкий уровень API, например Gingerbread.

### <a name="unifying-google-play-services"></a>Объединить службы Google Play

В более поздних версиях Xamarin.Android, мы отдача `aapt.exe` ресурсов версии для использования, поэтому данная проблема не исчезнет, нам.

Это означает, что нет особых причин иметь отдельные пакеты для Gingerbread/ICS/JellyBean/KitKat (тем не менее мы по-прежнему требуются отдельные привязки для Froyo так как она является полностью различных JAR-файлу).

Чтобы упростить для разработчиков, мы теперь одинаково наши компоненты и NuGet пакеты на два:

-   (Froyo) служб Google Play (привязывает `google-play-services-froyo.jar`)
-   Службы Google Play (привязывает `google-play-services.jar`)

### <a name="which-one-should-be-used"></a>Какой из них следует использовать?

В этом следует использовать службы Google Play. Единственная причина для использования пакета (Froyo) — Если вы активно используете Froyo. Единственной причиной, что это отдельный JAR-файлу существует от Google потому Froyo включен такой небольшой процент устройств, они сами принято решение запретить использование, поэтому JAR-файлу замороженный, неподдерживаемую моментальный снимок службы Google Play.

### <a name="note-about-gingerbread"></a>Примечание об Gingerbread

Gingerbread не имеет по умолчанию поддерживает фрагмент, и по этой причине некоторые классы в привязке не будет доступна в приложение на устройстве Gingerbread во время выполнения. Классы, такие как `MapFragment` не будут работать на Gingerbread и их поддержка вариант должен использоваться вместо `SupportMapFragment`. Именно разработчику известно, что именно использовать. Эта несовместимость указывается с Google в своей документации службы Google Play.

### <a name="what-happens-to-the-old-componentsnugets"></a>Что происходит с старых компонентов или NuGet?

Так как они больше не нужны, у нас есть отключено или Delisted следующие компоненты и NuGets:

-   Службы Google Play (Gingerbread)
-   Службы Google Play (JellyBean)
-   Службы Google Play (KitKat)

Существующий _служб Google воспроизведения (ICS)_ компонент/Nuget был переименован в _службы Google Play_ и будет храниться в актуальном состоянии Забегая вперед. Все проекты, ссылки на один из пакетов отключено или Delisted должен быть обновлен для использования данного объекта.

Отключенные компоненты по-прежнему существуют и должны быть восстановлены для проектов, которые они по-прежнему ссылаться в, чтобы избежать нарушения их. Аналогичным образом delisted пакеты NuGet по-прежнему существует и может быть восстановлена. Они не обновляются Забегая вперед.
