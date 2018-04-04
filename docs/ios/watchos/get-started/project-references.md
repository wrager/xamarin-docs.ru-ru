---
title: Ссылки проекта
description: Описание связи между приложения iOS, приложение watch и расширении.
ms.prod: xamarin
ms.assetid: C366E062-C33D-406A-B3FF-CBE82E5D1E7E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/13/2016
ms.openlocfilehash: f3573e8b578ca567ea9d7360eb132aead4c24f37
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="project-references"></a>Ссылки проекта

_Описание связи между приложения iOS, приложение watch и расширении._

Три проекты в решении watchOS *автоматически настроенного* ссылаться друг с другом определенным образом для watchOS 3 приложений следует создавать и объединенными правильно. Эти ссылки проекта и параметров идентификатора пакета описаны ниже для справки.

## <a name="project-references"></a>Ссылки проекта

Просмотр ссылок, дважды щелкнув ссылки на узлы для каждого проекта:

- **приложение для iPhone** ссылки **приложение Watch**

![](project-references-images/catalog-reference1.png "приложение для iPhone ссылается приложение Watch")

- **Просмотр приложения** ссылки **расширение приложения Контрольное значение**

![](project-references-images/catalog-reference2.png "приложение для iPhone ссылается приложение Watch")


 - **Расширении приложения** не ссылается ни один из других проектов

![](project-references-images/catalog-reference3.png "Посмотрите, как расширение приложения не ссылается на другие проекты")



## <a name="bundle-identifiers"></a>Идентификаторы пакета

Также необходимо убедиться в **идентификаторы набора** заданы правильно.
Все три проекта должно быть *же* префикс идентификатора, с проектами две контрольные значения, стандартные расширения `watchkitextension` и `watchkitapp`, как описано ниже (для **WatchKitCatalog** пример).

 - Проект единого Xamarin.iOS- `com.xamarin.WatchKitCatalog`

 - Проект расширения WatchKit- `com.xamarin.WatchKitCatalog.watchkitextension`

 - Проект приложения Контрольные значения- `com.xamarin.WatchKitCatalog.watchkitapp`

Также убедитесь, что они **Info.plist** правильность параметров:

 - Проект приложения Контрольное значение `WKCompanionAppBundleIdentifier` соответствует идентификатор набора приложений родительского или контейнера (ie. того, который выполняется на iPhone);

 - Проект расширения комплект средств для наблюдения за **идентификатор пакета WKApp** соответствующий идентификатор пакета в проект приложения контрольных значений.

Идентификаторы можно изменить, дважды щелкнув **Info.plist** файлы в каждом проекте.

Данный снимок является **расширении Watch** файл Info.plist, показывающая **приложение Watch** идентификатор также:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)
    
![](project-references-images/infoplist-extension.png "На этом снимке экрана — файл Info.plist расширения Контрольное значение")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
![](project-references-images/infoplist-extension-vs.png "На этом снимке экрана — файл Info.plist расширения Контрольное значение")

-----

На этом снимке экрана — **приложение Watch** файл Info.plist.
Текущего **ОС Контрольные значения** версия — 8.2, поэтому **цель развертывания** для приложения Контрольное значение должно быть **8.2**. Обратите внимание, что если у вас есть Xcode 6.3 установлены, это значение может быть задано до формата 8.3 его следует менять 8.2.

![](project-references-images/infoplist-watchapp.png "Контрольное значение файл Info.plist")

Целевой объект развертывания приложения Контрольное значение может отличаться от расширении и приложение iOS.

