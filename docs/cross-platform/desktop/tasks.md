---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Сравнение общих задач
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: b4d45ae61c5d7a7093d51c4440d11dd883c157a4
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="common-tasks-comparison"></a>Сравнение общих задач

| Задача | WPF | Xamarin.Forms |
|--- |--- |--- |
|Отображать сообщение на экране с кнопками|`MessageBox`|`Page.DisplayAlert`|
|Создать таймер|Класс `DispatcherTimer`|`Device.StartTimer` статический метод|
|Получить размер шрифта по умолчанию|`SystemFonts` статический класс|`Device.GetNamedSize` статический метод|
|Откройте URI или URL-адрес|`Process.Start`|`Device.OpenUri`|
|Откройте окно действие (список кнопки)|Н/Д|`Page.DisplayActionSheet`|
