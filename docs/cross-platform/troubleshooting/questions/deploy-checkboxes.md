---
title: Развертывание флажки отключена в диспетчере конфигурации
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: aaf675cd-d885-4dac-9754-77dbcaea3be9
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 12/02/2016
ms.openlocfilehash: c0f116565a2741c62a00ed2a255cfde8c57b8569
ms.sourcegitcommit: 6f7033a598407b3e77914a85a3f650544a4b6339
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="deploy-checkboxes-disabled-in-configuration-manager"></a>Развертывание флажки отключена в диспетчере конфигурации

С момента Xamarin 3.5 Xamarin.iOS проекты развертываются автоматически при каждом нажатии клавиши **запустить** кнопки панели инструментов или выбора **Отладка > Начать отладку** элемента меню. Необходимо задать нужный проект приложения Xamarin.iOS как **запускаемый проект** перед перед запуском одного из этих команд.

По этой причине **развернуть** флажки преднамеренно отключены в Visual Studio Configuration Manager для Xamarin.iOS проектов:

![](deploy-checkboxes-images/configuration.png "Visual Studio Configuration Manager отображается флажок «Развернуть» отключена для проекта Xamarin.iOS в Xamarin 3.5")

Это изменение исключает ошибку, которая может отображаться в более старых версиях Xamarin (версия 3.3 и более ранних версий), когда проект приложения Xamarin.iOS не было задано для развертывания:

![](deploy-checkboxes-images/error.png "Окно сообщения об ошибке: iPhoneApp1 проект должен быть развернут, прежде чем можно будет запустить. Убедитесь, что проект выбран для развертывания решения Configuration Manager.")
