---
title: "Атрибут Debuggable"
ms.topic: article
ms.prod: xamarin
ms.assetid: 3BE5EE1E-3FF6-4E95-7C9F-7B443EE3E94C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/05/2018
ms.openlocfilehash: db09ebe29b6c404bac892fd76389faf0b9e03d5b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="debuggable-attribute"></a>Атрибут Debuggable

<a name="Overview" />


Для отладки Android поддерживает протокол JDWP (Java Debug Wire Protocol). Эта технология позволяет некоторым средствам, например ADB, взаимодействовать с виртуальной машиной Java. Протокол JDWP очень важен на этапе разработки, но не забывайте отключить его перед публикацией приложения.

JDWP можно задать значением атрибута `android:debuggable` в приложении Android. Xamarin.Android предоставляет следующие методы для установки этого атрибута.

1.  Создайте файл `AndroidManifext.xml` и задайте в нем атрибут `android:debuggable`.
1.  Включите `ApplicationAttribute` в файл `.CS`, например так: `[assembly: Application(Debuggable=false)]` .


Если присутствуют одновременно `AndroidManifest.xml` и `ApplicationAttribute`, содержимое `AndroidManifest.xml` имеет более высокий приоритет, чем `ApplicationAttribute`.

Если не указано ни `AndroidManifest.xml`, ни `ApplicationAttribute`, то значение по умолчанию для атрибута `android:debuggable` зависит от того, создаются ли отладочные символы. Если символы отладки присутствуют, Xamarin.Android устанавливает для атрибута `android:debuggable` значение `true`.

Обратите внимание, что значение атрибута `android:debuggable` НЕ ВСЕГДА зависит от конфигурации сборки. Может случиться так, что для сборки выпуска атрибут `android:debuggable` имеет значение true.


## <a name="related-links"></a>Связанные ссылки

- [Приложения с атрибутом Debuggable на Android Market](http://labs.mwrinfosecurity.com/blog/2011/07/07/debuggable-apps-in-android-market/)
