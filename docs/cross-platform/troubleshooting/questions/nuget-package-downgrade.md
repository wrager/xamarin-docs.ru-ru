---
title: Понижение пакета NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.openlocfilehash: 2eec947ab748eccd45971054f2f5b232cf788cfa
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Понижение пакета NuGet

Visual Studio для Mac и Visual Studio имеется для выбора более старых версий пакетов и установка их автоматически. как и как работает обновление пакетов. Эти действия описаны ниже.

## <a name="visual-studio"></a>Visual Studio
1. Последовательно выберите пункты **Сервис > Диспетчер пакетов NuGet > консоль диспетчера пакетов**
2. Настроить проект под **проект по умолчанию**
3. Используйте следующий синтаксис:

    > Install-Package [PackageName]-версия [tab для меню версия]

Вы можете также скопируйте и вставьте точную команду со страницы пакета NuGet. Пример для Xamarin.Forms. [https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac
1. В проекте, щелкните правой кнопкой мыши «пакеты» и выберите **Добавление пакетов**
2. В searchbar чтобы найти необходимые пакеты можно использовать следующий синтаксис:

    `[PackageName] version:*`

### <a name="examples"></a>Примеры 
- Перечисляет все пакеты Xamarin.Forms: 

    `Xamarin.Forms version:`
- Перечисляет все пакеты 1.4.x Xamarin.Forms: 

    `Xamarin.Forms version:1.4`

*Примечание: Если добавить пробел между `version:` & номер версии, поиск будет вести себя как если бы, если не указана версия.*

