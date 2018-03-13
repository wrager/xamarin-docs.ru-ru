---
title: "Пользовательский интерфейс в iOS"
description: "Описание работы с iOS пользовательского интерфейса в приложении Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BB46561-F503-491E-A27C-7878E7EBE00B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: f456b54180d50cfc4b6b98ed8f3d4118c8397b37
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="user-interface-in-ios"></a>Пользовательский интерфейс в iOS

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




