---
title: Пользовательские интерфейсы в iOS
description: Документ содержит ссылки на руководства, описывающие, как создавать пользовательские интерфейсы в приложении Xamarin.iOS. Связанные направляющие охватывают API внешнего вида, создания объектов пользовательского интерфейса, параметры макета и многое другое.
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: a51d3f57106a282ed72b45dedf356739244e247f
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790371"
---
# <a name="user-interfaces-in-ios"></a>Пользовательские интерфейсы в iOS

## <a name="appearance-apiintroduction-to-the-appearance-apimd"></a>[API внешнего вида](introduction-to-the-appearance-api.md)

iOS позволяет много visual атрибуты элементов управления пользовательского интерфейса с помощью API-интерфейсы UIAppearance применять к.

## <a name="creating-user-interface-objectsiosuser-interfaceios-uicreating-ui-objectsmd"></a>[Создание объектов пользовательского интерфейса](~/ios/user-interface/ios-ui/creating-ui-objects.md)

Apple группы связанных функциональные элементы в «платформы», которые приравниваются к Xamarin.iOS пространства имен. `UIKit` представляет пространство имен, содержащее все элементы управления пользовательского интерфейса для операций ввода-вывода.

## <a name="layout-optionsiosuser-interfaceios-uilayout-optionsmd"></a>[Параметры макета](~/ios/user-interface/ios-ui/layout-options.md)

Существуют две различные механизмы управления макетом, если размер или поворачивать представления: автоматическое изменение размера и Autolayout.

## <a name="providing-haptic-feedbackiosuser-interfaceios-uihaptic-feedbackmd"></a>[Обеспечение обратной связи Haptic](~/ios/user-interface/ios-ui/haptic-feedback.md)

В этой статье описаны новые типы доступны в iOS 10 и способы их реализации в Xamarin.iOS haptic обратной связи.

## <a name="working-with-the-ui-threadiosuser-interfaceios-uiui-threadmd"></a>[Работа с потоком пользовательского интерфейса](~/ios/user-interface/ios-ui/ui-thread.md)

Код следует добавлять только поток изменения пользовательского интерфейса элементов управления из главного (или пользовательского интерфейса). Все обновления пользовательского интерфейса, которые происходят в другом потоке (например, обратный вызов или фоновый поток) не может получить выведено на экран или даже привести к сбою.




