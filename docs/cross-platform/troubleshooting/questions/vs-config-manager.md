---
title: Почему Visual Studio не включает указанную библиотеку проекта в моей сборкой?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: b9009db8-e716-43aa-b40e-6f28a8eb1b82
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: 8a35c8e0cd03d5ff07405647b08275ef9ca3758d
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="why-doesnt-visual-studio-include-my-referenced-library-project-in-my-build"></a>Почему Visual Studio не включает указанную библиотеку проекта в моей сборкой

Visual Studio использует **Configuration Manager** для определения, какие проекты в решении, автоматически включаются в заданной конфигурации сборки или развертывания.

Некоторые шаблоны, которые создаются в проекте библиотеки, на которую указывает ссылка, уже будет иметь указанную библиотеку, включенных в конфигурацию; Однако в противном случае его необходимо настроить вручную.

## <a name="how-to-use-the-configuration-manager"></a>Использование диспетчера конфигурации

1. Откройте **сборки > Configuration Manager**
2. Выберите конфигурацию, чтобы настроить, например **отладка | iPhone**
3. Установите флажки для проектов, которые нужно включить.

> [!NOTE]
> Неактивные поля обрабатываются автоматически и не должен необходимые изменения.

Демонстрация следующие действия. [http://screencast.com/t/zLoQOpEn](http://screencast.com/t/zLoQOpEn)
