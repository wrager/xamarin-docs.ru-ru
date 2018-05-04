---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Сравнение общих задач
ms.technology: xamarin-crossplatform
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 84b3edc1a8cbc9ad642d94b457321786fae5fe70
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="common-tasks-comparison"></a>Сравнение общих задач

| Задача | WPF | Xamarin.Forms |
|--- |--- |--- |
|Отображать сообщение на экране с кнопками|`MessageBox`|`Page.DisplayAlert`|
|Создать таймер|Класс `DispatcherTimer`|`Device.StartTimer` статический метод|
|Получить размер шрифта по умолчанию|`SystemFonts` статический класс|`Device.GetNamedSize` статический метод|
|Откройте URI или URL-адрес|`Process.Start`|`Device.OpenUri`|
|Откройте окно действие (список кнопки)|Н/Д|`Page.DisplayActionSheet`|
