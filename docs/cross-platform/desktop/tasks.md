---
ms.assetid: 0B45BF03-145B-43B2-AFD9-5A0EAB1E63A9
title: Сравнение общих задач
description: В этом документе сравнивается выполнение различных задач на WPF и Xamarin.Forms. Он выполняет поиск кнопки, таймеры, размеры шрифтов, открыв URI и отображение листом действия.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 2991e74ec59e3c43c665941706c2d9ac94bfb9ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780480"
---
# <a name="common-tasks-comparison"></a>Сравнение общих задач

| Задача | WPF | Xamarin.Forms |
|--- |--- |--- |
|Отображать сообщение на экране с кнопками|`MessageBox`|`Page.DisplayAlert`|
|Создать таймер|Класс `DispatcherTimer`|`Device.StartTimer` статический метод|
|Получить размер шрифта по умолчанию|`SystemFonts` статический класс|`Device.GetNamedSize` статический метод|
|Откройте URI или URL-адрес|`Process.Start`|`Device.OpenUri`|
|Откройте окно действие (список кнопки)|Н/Д|`Page.DisplayActionSheet`|
